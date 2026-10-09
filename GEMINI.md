# Topograph company evidence

Topograph connects to official business registers. The data server supports company search, company data and source documents, coverage and pricing, request retrieval and monitoring. The Wizard provides product documentation and integration guidance.

Sign in to the data server with your own Topograph organisation. A subscription with API access is required: https://topograph.co. Use `/mcp auth topograph` if sign-in is needed. The Wizard has its own sign-in: `/mcp auth topograph-wizard`.

Before paid retrieval, check country coverage and prices and obtain the user's spending approval. Set `max_cost_credits` to their approved budget where supported. Identify the exact company before buying data or documents. Do not start monitoring without the user's request.

Use verification mode for authoritative company evidence. Explain field-level source provenance, freshness and coverage limits. Distinguish live register data, cached data and AI-derived analysis. Missing ownership information does not prove the absence of beneficial owners. Topograph provides evidence for human review; it does not issue legal or compliance approvals.

Documentation: https://docs.topograph.co/guides/mcp

For ownership graphs, use get_graph with the user-approved max_cost_credits limit. The total depends on companies discovered, so explain that the limit is a ceiling rather than an exact quote. Use continue_graph to expand only the selected budget_truncated company node IDs under the returned main_request_id, with fresh approval of each continuation limit. Poll its request_id for progress, then get_request on main_request_id to see the merged graph. Already purchased company data is deduplicated; never claim missing ownership information proves there are no beneficial owners. Graph traversal requires the live environment and verification mode.
