# Topograph Claude Code plugin

Claude Code plugin that adds:

- Two MCP servers, both served by the NestJS API in `apps/api`
  (`apps/api/src/features/mcp/`):
  - `data`, the **Topograph MCP** (`https://mcp.topograph.co/mcp`): query
    company data from official business registers. Paid tools are billed like
    the REST API, with a monthly MCP spending cap per person.
  - `wizard`, the **Topograph Wizard** (`https://mcp.topograph.co/wizard`):
    live coverage, pricing, docs, OpenAPI and code samples for building an
    integration. Its former URL, `https://api.topograph.co/designer-mcp`,
    keeps working.
- Slash commands: `/topograph:lookup`, `/topograph:search`,
  `/topograph:cost`, `/topograph:integrate`, `/topograph:add-country`.
- Skills: `topograph-company-lookup`, `topograph-integration` and
  `topograph-country-coverage`, which auto-activate when Claude detects the
  user is looking up a company or working with Topograph.
- An ambient `CLAUDE.md` fragment that loads when the plugin is enabled.

Claude Code names the tools `mcp__plugin_topograph_data__<tool>` and
`mcp__plugin_topograph_wizard__<tool>`.

## Install

Add the public marketplace:

```
/plugin marketplace add getsemaphore/topograph-mcp-library
```

Install the plugin:

```
/plugin install topograph@topograph
```

Then run `/mcp` and sign in to each server. Both use a standard OAuth 2.1
PKCE flow against Topograph's sign-in.

- **Wizard:** any Topograph login works. No invite or organisation is needed
  for catalog browsing, the pricing simulator and personalized quotes.
- **Data:** the sign-in screen asks which organisation (environment) the
  agent acts for. The organisation needs API access, like the REST API.
  Requests appear in the app's request history and are billed to that
  organisation's wallet.

To use an API key instead of OAuth for the data server, add it by hand:

```
claude mcp add --transport http topograph https://mcp.topograph.co/mcp \
  --header "Authorization: Bearer $TOPOGRAPH_API_KEY"
```

Full guides: https://docs.topograph.co/guides/mcp (data) and
https://docs.topograph.co/guides/topograph-mcp (Wizard).

## Local dev install

For local testing, copy this marketplace structure to a local directory and
override `.mcp.json` to point at your local API, for example
`http://localhost:<api-port>/wizard` and `http://localhost:<api-port>/mcp`.
Then install from that local marketplace path.

## Distribution

This source lives in the monorepo so it can ship with the rest of Topograph.
The public marketplace repo at `github.com/getsemaphore/topograph-mcp-library`
mirrors this directory under `plugins/topograph/`, kept in sync by
`.github/workflows/sync-claude-plugin.yml` on every push to `main`.

Use a local marketplace copy to verify the install flow before pushing.
