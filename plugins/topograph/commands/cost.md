---
description: Estimate Topograph cost for a country + datapoints + volume.
argument-hint: <country-code> <datapoints> [volume]
---

Estimate the cost of using Topograph for: $ARGUMENTS

Interpret the arguments as `<country-code> <datapoints> [volume]`. Examples:

- `/topograph:cost FR company,ultimateBeneficialOwners 1000`
- `/topograph:cost DE shareholders 500`
- `/topograph:cost US_NY company` (no volume = unit cost)

Steps:

1. Parse the country code, datapoints (comma-separated), and optional volume.
2. Call `get_country(cc)` on the `topograph` MCP.
3. Compute the per-company cost:
   - For each requested datapoint, find the cheapest `dataBlock` that
     covers it (i.e. its `datapoints[]` includes the datapoint).
   - Collect the *unique* blocks needed — when one block covers several
     datapoints, count it once.
   - Sum `manifest.skus[<sku>].priceFixed` across those unique blocks. That's
     the per-company cost in credit cents.
4. Render a short summary:
   - Per-call cost in EUR/USD
   - Total monthly cost (per-call × volume) if volume was provided
   - Any caveats: variable-priced SKUs (return a range, not a number),
     datapoints not available in this country, register flakiness.

If a datapoint isn't covered by any block, surface that explicitly — don't
silently exclude it from the quote.
