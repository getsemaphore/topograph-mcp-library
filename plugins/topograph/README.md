# Topograph Claude Code plugin

Claude Code plugin that adds:

- An MCP registration pointing at `https://www.topograph.co/mcp` (served by
  the `/mcp` App Router route in `apps/landing`).
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

The plugin points at the OAuth-protected MCP endpoint:
`https://www.topograph.co/mcp`.

The MCP client will ask you to sign in with Topograph. It does not require your
REST API key.

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
