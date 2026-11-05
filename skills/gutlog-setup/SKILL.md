---
name: gutlog-setup
description: Connect an assistant, a machine or CI to Gutlog — add the hosted MCP server to ChatGPT, Claude, Claude Code, Codex or Cursor, sign the CLI in through the browser, or create a scoped API key for a script. Use for "connect Gutlog", "set up the Gutlog MCP server", "give our backend a Gutlog key", "Gutlog CLI login"; do not use for working inside an organization once connected (gutlog) or for plans and billing (gutlog-billing).
---

# Connect to Gutlog

The quickest path for a person: open https://getgutlog.com/setup?ref=setup-plugin, or tell their
assistant "Set up Gutlog for me: read https://getgutlog.com/setup.md?ref=setup-plugin and follow
it." That page has the clicks and deep links for every client.

## The plugin

Install the plugin, then use any tool: your agent opens a Gutlog sign-in in the
browser and keeps the connection. In Claude Code, `/mcp` shows the connection
and signs in again if needed. Codex, Cursor and the Claude and ChatGPT apps sign
in the same way from their connector settings. For scripts, CI and the SDK,
create an API key in the dashboard and keep it as a secret in that environment;
the plugin never uses it.

## The CLI

```bash
npx @meterapp/gutlog login
```

opens the browser, the person approves the code, and the CLI keeps a key for
the organization they choose. On a machine without a browser:
`npx @meterapp/gutlog login --no-browser` prints the code and the link. For CI,
create a key in the dashboard and keep it as a repository secret; how the CLI
and the SDK pick it up is in https://getgutlog.com/docs?ref=setup-plugin.

## The MCP server

Hosted at `https://getgutlog.com/api/mcp` (streamable HTTP, OAuth sign-in). In
Claude Code:

```bash
claude mcp add --transport http gutlog https://getgutlog.com/api/mcp
```

then `/mcp` to sign in. On a machine without a browser, the person adds the
server themselves with their own key, as shown in
https://getgutlog.com/docs/mcp?ref=setup-plugin#claude-code. A client that only
speaks stdio can use the CLI's bridge: `npx @meterapp/gutlog mcp`. Prices and
plans need no sign-in at `https://getgutlog.com/api/mcp/catalog`.

## API keys

A person creates keys in the dashboard (Settings → API keys) or with
`create_api_key`. A key acts with its creator's role (an owner's key acts as
admin) and can be narrowed with scopes: `meals:read`, `gut:write`,
`meals:*`, `*:read`, and `billing:purchase` (never implied). Keys belong to
the organization and keep working if their creator leaves; admins revoke them
with `revoke_api_key`. The full key (`gutlog_…`) is shown once: store it as a
secret, never in client-side code.
