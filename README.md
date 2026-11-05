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
| `gutlog-setup` | connect an assistant, the CLI or an API key |
| `gutlog-billing` | plans, daily analyses, upgrades |
| `pantry` | the kitchen inventory: what is on hand and where, what expires, the shopping list |

Install in Claude Code:

```
/plugin marketplace add MeterApp/gutlog-plugins
/plugin install gutlog@gutlog
```

Or connect any MCP client to `https://getgutlog.com/api/mcp` (setup for every client:
https://getgutlog.com/setup?ref=plugin). This directory is published from the Gutlog source repository;
open issues at https://getgutlog.com/feedback?ref=plugin.
