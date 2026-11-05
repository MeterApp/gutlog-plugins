---
name: gutlog-setup
description: Connect an assistant, a machine or CI to Gutlog — add the hosted MCP server to ChatGPT, Claude, Claude Code, Codex or Cursor, sign the CLI in through the browser, or create a scoped API key for a script. Use for "connect Gutlog", "set up the Gutlog MCP server", "give our backend a Gutlog key", "Gutlog CLI login"; do not use for working inside an organization once connected (gutlog) or for plans and billing (gutlog-billing).
---

# Connect to Gutlog

The quickest path for a person: open https://getgutlog.com/setup?ref=setup-plugin, or tell their
assistant "Set up Gutlog for me: read https://getgutlog.com/setup.md?ref=setup-plugin and follow
it." That page has the clicks and deep links for every client.

## The CLI

```bash
npx @meterapp/gutlog login
```

opens the browser, the person approves the code, and the CLI saves an API key
for the organization they choose. On a machine without a browser:
`npx @meterapp/gutlog login --no-browser` prints the code and the link. In CI,
set `GUTLOG_API_KEY` from a repository secret instead.

## The MCP server

Hosted at `https://getgutlog.com/api/mcp` (streamable HTTP, OAuth sign-in). In
Claude Code:

```bash
claude mcp add --transport http gutlog https://getgutlog.com/api/mcp
```

then `/mcp` to sign in. A client that only speaks stdio can use the CLI's
bridge: `npx @meterapp/gutlog mcp`. Prices and plans need no sign-in at
`https://getgutlog.com/api/mcp/catalog`.

## API keys

A person creates keys in the dashboard (Settings → API keys) or with
`create_api_key`. A key acts with its creator's role (an owner's key acts as
admin) and can be narrowed with scopes: `meals:read`, `gut:write`,
`meals:*`, `*:read`, and `billing:purchase` (never implied). Keys belong to
the organization and keep working if their creator leaves; admins revoke them
with `revoke_api_key`. The full key (`gutlog_…`) is shown once: store it as a
secret, never in client-side code.
