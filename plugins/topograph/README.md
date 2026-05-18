# Topograph Claude Code plugin

Claude Code plugin that adds:

- An MCP registration pointing at `https://api.topograph.co/designer-mcp`
  (served by the NestJS `designer-mcp` controller in `apps/api`).
- Slash commands: `/topograph:search`, `/topograph:cost`,
  `/topograph:integrate`, `/topograph:add-country`.
- Skills: `topograph-integration` and `topograph-country-coverage` that
  auto-activate when Claude detects the user is working with Topograph.
- An ambient `CLAUDE.md` fragment that loads when the plugin is enabled.

## Install

Add the public marketplace:

```
/plugin marketplace add getsemaphore/topograph-mcp-library
```

Install the plugin:

```
/plugin install topograph@topograph
```

The plugin points at the Clerk-protected MCP endpoint:
`https://api.topograph.co/designer-mcp`.

The MCP client will run a standard OAuth 2.1 PKCE flow against Topograph's
Clerk-hosted sign-in. Any Clerk user works — no invite or org membership is
required for catalog browsing, pricing simulator, and personalized quotes.
Production REST API access (`/v2/search` and `/v2/company` — the latter
also serves document orders via the `documents` array) is separate and
requires being invited to a customer org by sales.

## Local dev install

For local testing, copy this marketplace structure to a local directory and
override `.mcp.json` to point at your local landing server, for example
`http://localhost:3000/mcp`. Then install from that local marketplace path.

## Distribution

This source lives in the monorepo so it can ship with the rest of Topograph.
The public marketplace repo at `github.com/getsemaphore/topograph-mcp-library`
mirrors this directory under `plugins/topograph/`, kept in sync by
`.github/workflows/sync-claude-plugin.yml` on every push to `main`.

Use a local marketplace copy to verify the install flow before pushing.
