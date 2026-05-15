---
description: Start a guided Topograph integration walkthrough for a country or countries.
argument-hint: [country-codes]
---

Run the `integrate-topograph` MCP prompt on the `topograph` server.

If $ARGUMENTS is provided, pass it as the `countries` argument (comma-separated
country codes). Otherwise leave both arguments empty and let the user clarify
during the walkthrough.

The prompt will:

1. Verify Topograph covers the user's desired countries / datapoints
2. Estimate cost for their expected volume
3. Explain authentication
4. Generate a concrete code sample in their preferred language
5. Suggest the first end-to-end call to verify the integration

If the user already has an integration and just wants to add a new country,
suggest `/topograph:add-country <cc>` instead.
