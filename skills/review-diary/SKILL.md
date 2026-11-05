---
name: review-diary
description: Read, summarize, audit and export the person's Gutlog food diary as data — calories, macros or any nutrient over any range against their goals, what they eat most, gaps and unconfirmed items to fix, and a full export with a data dictionary. Use for "how was my week", "average protein this month", "what do I eat most", "how often do I have garlic", "audit my diary", "set my calorie goal", "export my data"; do not use for logging a new meal (log-food) or for gut symptoms and triggers (gut-check).
---

# Review the diary

The diary is a dataset: every row says who logged it, from where, where each
number came from (label, database, photo estimate or an edit) and how
confident it is. Nutrients are **integers**: energy in millikilocalories,
macros and most minerals in milligrams, some vitamins in micrograms; every
`*_display` field has kcal and grams for people. The full data dictionary is
the MCP resource `gutlog://data-dictionary`.

## Answer a question

| They ask | Call |
| --- | --- |
| One day | `get_day` |
| A week or a month, meal by meal | `get_diary` (`from`, `to`; at most 31 days a call) |
| Totals, averages, "am I hitting my goals" | `get_nutrition_summary` (`nutrients`, `group_by` day, week, month or meal_type) |
| What they eat most, top protein sources, everything with an ingredient | `query_foods` (`query`, `ingredient`, `sort` times, recent or a nutrient) |
| Set or change goals | `get_goals`, `set_goals` (calories, protein_g, carbs_g, fat_g, fiber_g, sodium_mg…) |

Averages are per **logged** day. When `days_logged` is well under
`days_in_range`, say so before drawing conclusions. Round for people; keep the
integers when you compute.

Every read covers the whole history on every plan.

## Audit before you trust it

`audit_diary` (Plus; default the last 14 days) returns findings, most important
first: missing meals, meals flagged for review and possible ingredients
nobody confirmed, low-confidence matches, implausible portions, days with
meals but no symptom log, likely duplicates. Each finding has
`fix: { op, input, how }`:

1. Ask the person what `how` says to ask (one or two findings at a time).
2. Call `fix.op` with `fix.input` plus what they told you. If
   `fix.available` is false, that op is not on this server yet: tell them
   what needs changing instead.
3. Run `audit_diary` again to confirm.

On Free it answers 402 with an upgrade link: mention it once, then run the
same checks yourself from `get_diary` (days with under two meals, meals with
`needs_review`, ingredients with status `suggested`, items over 3× a normal
portion, days with meals and no symptom log, near-identical meals minutes
apart).

## Export

`export_data` (every plan) returns signed links that expire in an hour to
`export.json`, one CSV per table and `README.md` (the data dictionary). Give
the links only to the person. `delivery: "inline"` returns README and JSON in
the response for a short range.

## Surveys

A read may include `survey`: one optional question. After answering what they
asked, ask it once in your own words; record their answer with
`answer_survey`. If they skip it, drop it.

## Rules

- Never give medical or diet prescriptions; goals are the person's own
  numbers (suggest a registered dietitian for medical diets).
- Health data stays in Gutlog: never paste it into feedback or other tools.
