# Gutlog plugin

The [Gutlog](https://getgutlog.com?ref=plugin) plugin for Claude Code, Codex, Cursor and other
agents, by Meter: the hosted Gutlog MCP servers and nine skills.

| skill | for |
| --- | --- |
| `gutlog` | what Gutlog is, today's diary, the setup checklist, and which skill to use |
| `log-food` | log meals from a photo, a sentence, a barcode or a label; fix them after |
| `review-diary` | summaries against goals, what you eat most, the diary audit, exports |
| `gut-check` | symptom check-ins and patterns (hypotheses, never diagnoses) |
| `elimination-diet` | low-FODMAP and single-food tests, safety screen first |
| `clean-check` | clean-ingredient verdicts with evidence levels, organic advice, swaps |
| `gutlog-setup` | connect an assistant or the CLI, and keys for scripts and CI |
| `gutlog-billing` | plans, daily analyses, upgrades |
| `pantry` | the kitchen inventory: what is on hand and where, what expires, the shopping list |

Install in Claude Code:

```
/plugin marketplace add MeterApp/gutlog-plugins
/plugin install gutlog@gutlog
```

Install the plugin, then use any tool: your agent opens a Gutlog sign-in in the browser and
keeps the connection. In Claude Code, `/mcp` shows the connection and signs in again if needed.
Codex, Cursor and the Claude and ChatGPT apps sign in the same way from their connector
settings. For scripts, CI and the SDK, create an API key in the dashboard and keep it as a
secret in that environment; the plugin never uses it.

Or connect any MCP client to `https://getgutlog.com/api/mcp` (setup for every client:
https://getgutlog.com/setup?ref=plugin). This directory is published from the Gutlog source repository;
open issues at https://getgutlog.com/feedback?ref=plugin.

Privacy: [Privacy policy](https://getgutlog.com/privacy?ref=plugin) · Terms: [Terms of service](https://getgutlog.com/terms?ref=plugin) · Support: [Docs](https://getgutlog.com/docs?ref=plugin)
