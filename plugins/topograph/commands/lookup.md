---
description: Look up a real company with the Topograph MCP (search first, then fetch only what you ask for).
argument-hint: <company name or identifier> [country] [what you want to know]
---

Look up this company with the `data` MCP server (the Topograph MCP): $ARGUMENTS

Follow the `topograph-company-lookup` skill:

1. Work out the country. If $ARGUMENTS does not make it clear, ask before
   calling anything. US companies are per state (`US-DE`, `US-NY`, ...).
   Only if the user does not know the country, offer
   `search_companies_worldwide` (beta, billed when it finds one match).
2. Call `search_companies` (free) and confirm the right entity with the user
   when there is more than one plausible match.
3. Pick the narrowest datapoints that answer the question. With no question
   given, fetch `company` only.
4. Call `estimate_cost` (free) and tell the user the price before the paid
   `get_company` call. Pass `max_cost_credits`.
5. If the result comes back with a `request_id` and pending datapoints, poll
   with `get_request` (free). Never repeat `get_company`, which bills again.
6. Never order documents unless the user explicitly agrees to the price.
7. Summarize the answer in a short table and state what the lookup cost.

If $ARGUMENTS is empty, ask which company (name or registration number) and
which country.

If the `data` server is not connected, tell the user to run `/mcp` and sign in
to `data`, or to add the server with an API key (see
https://docs.topograph.co/guides/mcp).
