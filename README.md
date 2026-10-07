# Topograph Claude Code marketplace (monorepo source)

This directory is the **canonical source** for the Topograph Claude Code
plugin and marketplace. CI mirrors this tree to the public repo
`github.com/getsemaphore/topograph-mcp-library` on every push to `main`.

## Layout

```
packages/claude-plugin/
├── .claude-plugin/
│   └── marketplace.json       # marketplace metadata (mirrors to repo root)
├── plugins/
│   └── topograph/              # the plugin
│       ├── .claude-plugin/
│       │   └── plugin.json    # plugin metadata
│       ├── .mcp.json          # MCP servers: wizard (mcp.topograph.co/wizard) + data (mcp.topograph.co/mcp)
│       ├── CLAUDE.md          # ambient context
│       ├── commands/          # slash commands
│       ├── skills/            # auto-activating skills
│       └── README.md
└── README.md                   # this file
```

When pushed to the public repo, the `packages/claude-plugin/` contents become
the repo root, so installs look like:

```
/plugin marketplace add getsemaphore/topograph-mcp-library
/plugin install topograph@topograph
```

## Editing the plugin

Edit files under `plugins/topograph/`. Both MCP servers live in
`apps/api/src/features/mcp/` and are served by `apps/api` on
`https://mcp.topograph.co`: the Topograph MCP (company data) at `/mcp` and the
Topograph Wizard (catalog, pricing, docs) at `/wizard`. The Wizard's former
URL, `https://api.topograph.co/designer-mcp`, stays as a permanent alias.

After editing, push to `main` — the GitHub workflow at
`.github/workflows/sync-claude-plugin.yml` mirrors changes to the public
repo, which is what users `/plugin install` from.

## Local testing (without pushing)

To test the plugin before publishing, create a local marketplace directory with
the same structure as this folder and point `plugins/topograph/.mcp.json` at a
local API, for example `http://localhost:<api-port>/wizard` and
`http://localhost:<api-port>/mcp`.

Then install from the local marketplace path:

```
/plugin marketplace add /absolute/path/to/local/topograph-mcp-library
/plugin install topograph@topograph
```

## CI mirror

See `.github/workflows/sync-claude-plugin.yml`. On every push to `main` that
touches `packages/claude-plugin/**`, the workflow rebuilds the public repo's
content from this directory and pushes.

Requires a repo secret `PLUGINS_REPO_PUSH_TOKEN` — a fine-grained PAT
with `Contents: Read and write` permission on `getsemaphore/topograph-mcp-library`.

## Versioning

The plugin's version lives in `plugins/topograph/.claude-plugin/plugin.json`.
Bump it when shipping breaking changes. The marketplace itself doesn't carry
a version; clients track the underlying repo's `main` branch.
