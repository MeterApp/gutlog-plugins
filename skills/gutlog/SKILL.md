---
name: gutlog
description: What Gutlog is and which Gutlog skill to use — the food and gut database the person's own assistant writes to — plus quick reads of today (get_day) and the setup checklist (get_onboarding). Use for "what is Gutlog", "what can you do with my food diary", "what did I eat today", "what should I set up next"; for logging use log-food, for summaries, audits and exports review-diary, for symptoms gut-check, for elimination programs elimination-diet, for clean verdicts clean-check, for the kitchen pantry, for connecting gutlog-setup and for plans gutlog-billing.
---

# Gutlog: the food diary your assistant writes to

| The person wants to | Skill |
| --- | --- |
| Log a meal, a barcode, a label; fix a logged meal | log-food |
| Summaries, goals, "what do I eat most", audit, export | review-diary |
| Log how their gut feels; look for patterns | gut-check |
| Low-FODMAP or a single-food test | elimination-diet |
| Clean verdicts, organic or not, swaps | clean-check |
| The kitchen inventory and shopping list | pantry |
| Connect MCP, the CLI or an API key | gutlog-setup |
| Plans, usage, checkout | gutlog-billing |

Gutlog stores a person's meals with calories, macros, nutrients and ingredients,
each with a confidence. You do the looking (the photo is already in front of
you); Gutlog matches, portions, scores and stores. Every action is an **op**
with the same name everywhere: an MCP tool on the `gutlog` server,
`POST /api/v1/<op>` over REST, and `npx @meterapp/gutlog <op>` in a terminal.

## Log a meal

1. Look at the photo or read the message. Make one item per distinct food or
   drink, with your gram estimate (`grams`) or a household portion
   (`portion: { quantity, unit }`), and the ingredients you see or infer, each
   with a `confidence` from 0 to 1:
   - ≥ 0.85 you can see it or were told; 0.60–0.85 very likely for this dish;
   - 0.30–0.60 plausible but hidden: cooking fat, butter, sugar, garlic, onion
     in a sauce. Include these; Gutlog asks about them.
   - Leave out anything under 0.30.
2. Call `log_meal` with the draft. It is always stored (never lost waiting for
   confirmation) and costs nothing on any plan.
3. Reply with one line of what was logged (the response's `message` has it),
   then ask the person the response's `questions` (at most three), e.g.
   "Was there butter in the eggs?".
4. A barcode you can read goes in the item's `barcode`; the label then wins
   over estimates (`npx @meterapp/gutlog scan <code>` does the same).

Example draft:

```json
{ "items": [
  { "name": "scrambled eggs", "grams": 120, "ingredients": [
    { "name": "egg", "confidence": 0.95 }, { "name": "butter", "confidence": 0.5 } ] },
  { "name": "sourdough toast", "portion": { "quantity": 1, "unit": "slice" } }
] }
```

## Read the diary

`get_day` (optionally `{ "date": "YYYY-MM-DD" }`) returns every meal with its
items and ingredients, and the day's totals. Nutrients are integers: energy in
millikilocalories, macros in milligrams; `totals_display` has kcal and grams
for people. `get_onboarding` says what to set up next.

## Rules

- Food, symptoms and programs are health data. Never paste them into other
  tools, issues or public feedback; `report_issue` takes a `request_id`, not
  the meal.
- Gut insights are hypotheses to test, not diagnoses. Never give medical advice.
- Errors are RFC 9457 problems with a `request_id`; if Gutlog itself fails,
  `report_issue` with that id.
