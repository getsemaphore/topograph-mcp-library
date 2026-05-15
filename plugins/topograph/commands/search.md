---
description: Search Topograph's country catalog. Pass a country name, datapoint, document type, or register name.
argument-hint: <query>
---

Search the Topograph catalog for: $ARGUMENTS

Use the `topograph` MCP server:

1. Call `find_data` with the query above to surface countries, datapoints,
   documents, or registers that match.
2. If the result points at a specific country, follow up with `get_country` to
   show the full manifest (identifiers, data blocks, modes, limitations).
3. Summarize the result in a short table — don't dump raw JSON.

If the result set is empty, explain that the query didn't match anything in the
catalog and suggest `list_countries(region=...)` to browse instead.
