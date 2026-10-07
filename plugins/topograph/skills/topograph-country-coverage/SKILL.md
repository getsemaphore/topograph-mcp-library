---
name: topograph-country-coverage
description: Use when the user asks "does Topograph cover [country]?" or "what data does Topograph have for [country]?" or any country-specific Topograph coverage question. Fetches the live country manifest from the Topograph Wizard MCP (`wizard` server) instead of guessing or relying on training data.
---

# Topograph country coverage

When the user asks about coverage for a specific country:

1. Call `get_country(cc)` on the `wizard` MCP server (Topograph Wizard).
2. Summarize:
   - **Identifiers accepted** (with format + example)
   - **Data blocks offered** (block name → datapoints → modes available)
   - **Documents available** (trade register extract, UBO extract, etc.)
   - **Registers used** (the upstream sources)
   - **Known gaps** (`manifest.gaps`) — explain *why* something is missing
3. Use the right case for the country code: ISO 3166-1 alpha-2 for most
   countries, `US_XX` for US states (e.g. `US_NY`, `US_DE`).

If they ask about a country that returns "not found", suggest
`list_countries(region=...)` to browse what's available in the relevant region.

## Don't over-trust training data

Topograph adds countries frequently. The Wizard catalog is the source of truth.
Never claim "Topograph doesn't support X" without first calling the MCP: the
training cutoff is months behind what's live.

If only the `data` server is connected (the Topograph MCP for company data),
its free `get_country_coverage` tool returns the same coverage, prices and
identifier formats.
