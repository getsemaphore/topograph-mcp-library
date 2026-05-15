---
name: topograph-integration
description: Use when the user is integrating, calling, or evaluating the Topograph company-data API (any mention of topograph.co, @topograph/sdk, /v2/search, /v2/company, KYB/AML company lookups, signup flow / marketplace onboarding using business registers like INPI/INSEE/Companies House/Handelsregister/KVK). Pulls live coverage and pricing from the topograph MCP, and enforces the canonical onboarding + search-first methodology rather than guessing.
---

# Topograph integration

The user is working with Topograph — a company-data API covering 60+
countries and US states. You have an MCP server registered as `topograph`
exposing the public catalog, docs, and integration methodology rules.

## Before recommending anything

**ALWAYS fetch these two rules first** if the user is building any
identification or onboarding flow:

1. `topograph://rules/onboarding-methodology` — the canonical five-step flow
2. `topograph://rules/search-first-resolution` — turning fuzzy input into a
   canonical identifier

They prevent the two most common methodology mistakes:

- **Over-prefilling the signup form** with compliance-grade data (UBOs,
  shareholders, documents). Those belong in the audit step *after* the user
  has submitted, not in the live form.
- **Skipping search-first resolution.** Calling `/v2/company` with raw user
  input breaks ~30% of the time. Search is the cheap defensive layer that
  also returns `matchReason` for picker UX.

## The canonical five-step onboarding flow

```
1. RESOLVE — /v2/search with the user's typed input
             → use matchReason.matchType to auto-select or show picker
2. PREFILL — /v2/company with mode='onboarding'
             → narrow scope: company + (optionally) legalRepresentatives
3. SUBMIT  — user corrects + submits the form. Account created.
4. VERIFY  — /v2/company without mode (default verification)
             → asynchronous, after submit, for the compliance record
5. AUDIT   — separate calls for UBOs, shareholders, documents
             → asynchronous, after submit, for the audit trail
```

Steps 1–2 are user-facing and synchronous. Steps 4–5 are background jobs.

## What belongs in each phase

| Phase | Mode | Datapoints | Sync? |
|---|---|---|---|
| **Prefill (step 2)** | `onboarding` | `company` (+ `legalRepresentatives` if needed) | Sync |
| **Verify (step 4)** | `verification` | Same as prefill, fresh | Async after submit |
| **Audit (step 5)** | `verification` | `shareholders`, `ultimateBeneficialOwners`, documents | Async after submit |

**Never recommend `shareholders` or `ultimateBeneficialOwners` in step 2.**
They are not prefill data. They are compliance data.

## Workflow

**Step 1: Understand what they're building.**

Ask whether the integration is:

- A KYB / compliance audit (use verification mode end-to-end, no prefill UX)
- A marketplace / SaaS signup with verification (use the five-step flow)
- Pure search / lookup (search + onboarding-mode profile, no verification)

**Step 2: Resolve country coverage.**

Use the MCP tools — never guess:

- `list_countries` — what's supported (filter by region / capability)
- `get_country(cc)` — full manifest (identifiers, data blocks, modes, limitations)
- `find_data(query)` — search across the catalog

**Step 3: Recommend a phased flow with concrete API calls.**

For each country, produce the call sequence in order:

```
1. example_snippet(operation='search', cc=...)
2. example_snippet(operation='company_profile', cc=..., mode='onboarding')
3. (after submit) example_snippet(operation='company_profile', cc=...)     // verification
4. (after submit) example_snippet(operation='ubos', cc=...)
   (after submit) example_snippet(operation='order_document', cc=...)
```

**Step 4: Estimate cost honestly.**

Compute the quote from the manifest yourself: `get_country(cc)` returns
`dataBlocks[]` and `skus` together. For each requested datapoint, pick the
cheapest block that covers it. Sum the unique blocks' `skus[<sku>].priceFixed`.
Multiply by volume for the total. Variable-priced SKUs (`pricingMode:
"variable"`) carry `priceMarkup` + `usualPrice` instead — surface as a range.

Surface caveats:

- Variable-priced catalog items (cost reconciled at request time)
- Async datapoints (don't promise sub-second latency)
- Manifest limitations (`not_available`, `blocked`, `access_required`)
- Modes the block does NOT support (e.g. shareholders that are
  verification-only — don't quote them for an "onboarding prefill")

## When the user asks "verification or onboarding for my use case?"

Default to verification. Switch to onboarding only when:

- It's a signup/search flow where latency directly affects UX
- The data won't be used for compliance / AML / KYB record-keeping

Usually the right answer is **both**, in **different phases** — prefill in
onboarding mode at signup, verify in verification mode after submit.

## Authentication

The MCP client signs in to Topograph through OAuth. The user's application
still authenticates with the real Topograph REST API using its own API key. See
`topograph://rules/auth-setup`.

## Available rules resources (in priority order)

For deeper context, fetch:

1. `topograph://rules/onboarding-methodology` — **the canonical KYB flow**
2. `topograph://rules/search-first-resolution` — **the search-first pattern**
3. `topograph://rules/modes-onboarding-verification` — what each mode is
4. `topograph://rules/data-blocks` — billing semantics
5. `topograph://rules/documents` — when to order PDFs
6. `topograph://rules/integration-overview` — five-minute orientation
7. `topograph://rules/common-pitfalls` — thirteen things that bite people
8. `topograph://rules/data-product-model` — full mental model
9. `topograph://rules/auth-setup` — how to get and use an API key
10. `topograph://rules/country-coverage-matrix` — quick country list
