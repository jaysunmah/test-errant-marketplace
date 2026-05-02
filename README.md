# Test Errant Marketplace

A **private** test marketplace repo that intentionally contains a malformed
plugin entry, used to reproduce marketplace refresh failures end to end.

This is a sibling of
[`jaysunmah/test-marketplace`](https://github.com/jaysunmah/test-marketplace)
with one extra plugin entry — `errant-plugin` — whose `author.email` is set to
`not-an-email`. The shared schema validator in
`packages/cursor-plugins/src/manifest-parser.ts` rejects the entire
marketplace manifest because of this one invalid email, which is the same
class of failure surfaced as `Internal Server Error` from
`reindex-and-apply-team-marketplace-changes` in production.

## Structure

- **`/plugins`** - Internal plugins, including `errant-plugin`
- **`/external_plugins`** - Third-party plugin integrations

## Reproducing the failure

1. Add this repo to a team marketplace in the admin dashboard.
2. Trigger a manual refresh (or push to `main` if auto-refresh is enabled).
3. The refresh fails because `.claude-plugin/marketplace.json` contains an
   `errant-plugin` entry with an invalid `author.email`.

To restore the repo to a healthy state, edit the `errant-plugin` entry in
[`.claude-plugin/marketplace.json`](.claude-plugin/marketplace.json) and
either remove `author.email` or set it to a valid email.

## Plugin Structure

Each plugin follows a standard structure:

```
plugin-name/
├── .claude-plugin/
│   └── plugin.json      # Plugin metadata (required)
├── .mcp.json            # MCP server configuration (optional)
├── commands/            # Slash commands (optional)
├── agents/              # Agent definitions (optional)
├── skills/              # Skill definitions (optional)
└── README.md            # Documentation
```
