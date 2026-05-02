# errant-plugin

This plugin exists so the parent marketplace has a real local plugin entry that
points at a real directory. The interesting part lives one level up, in
[`/.claude-plugin/marketplace.json`](../../.claude-plugin/marketplace.json):
the `errant-plugin` entry there has an intentionally invalid `author.email`
(`not-an-email`).

This reproduces the marketplace refresh failure described in support: the
shared schema validator in
`packages/cursor-plugins/src/manifest-parser.ts` rejects the entire
`marketplace.json` because one entry's `author.email` is not a valid email,
which causes refreshes (manual and webhook-driven) to surface as
`Internal Server Error` from `reindex-and-apply-team-marketplace-changes`.

The plugin manifest in this directory is itself valid, so once the bad email
is corrected the rest of the marketplace should index normally.
