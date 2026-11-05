# Changelog

## 0.2.0 (2026-11-05)

- Sign-in is OAuth only: install the plugin, then use any tool and your agent
  opens a Gutlog sign-in in the browser. The optional API key setting and its
  header are gone from the Claude Code manifest, so its MCP servers are the
  same as `.mcp.json`. For scripts, CI and the SDK, create an API key in the
  dashboard and keep it as a secret there; the plugin never uses it.
- The listing links (privacy, terms, support) live in the Codex interface and
  the directory listing, no longer in the Claude Code manifest.
- The plugin ships no scripts: the structure check moved to the source
  repository and runs before each publish.

- Five skills: `log-food` (photo, sentence, barcode and label logging with
  tiered ingredient confidence, fixes and analysis ratings), `review-diary`
  (summaries against goals, what they eat most, the diary audit and exports),
  `gut-check` (symptom check-ins and looking back at bad days),
  `elimination-diet` (safety screen, low-FODMAP phases, single-food tests) and
  `clean-check` (the clean rubric with evidence levels, organic guidance).
  `gutlog` becomes the overview that points to them.
- New ops the skills use: `get_diary`, `get_nutrition_summary`, `query_foods`,
  `audit_diary`, `export_data`, `get_goals`, `set_goals`, `get_survey`,
  `answer_survey`; the `gutlog://data-dictionary` MCP resource.

- The `pantry` skill: the kitchen inventory (fridge, freezer, cupboards), expiry
  and freeze suggestions, raw ingredients on hand and the shopping list.

## 0.1.0 (2026-11-03)

- First release: the `gutlog`, `gutlog-setup` and `gutlog-billing` skills and the
  hosted Gutlog MCP servers (account and catalog). `log_meal` and `get_day` are
  the first product ops; food, clean, gut and program skills follow
  (`log-food`, `review-diary`, `gut-check`, `elimination-diet`, `clean-check`).
