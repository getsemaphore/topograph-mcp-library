---
description: Add support for a new country to an existing Topograph integration.
argument-hint: <country-code>
---

Run the `add-country` MCP prompt on the `topograph` server with `cc=$ARGUMENTS`.

The prompt will:

1. Pull the full manifest for the country
2. Flag identifier shape differences against typical integrations
3. List which datapoints / blocks are offered, in which modes, and any gaps
4. Show a code snippet for the user's preferred language
5. Estimate cost change against the user's typical volume

If $ARGUMENTS is empty, ask the user which country they want to add before
firing the prompt.
