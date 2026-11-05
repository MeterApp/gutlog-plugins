---
name: clean-check
description: Say how clean a packaged food, meal or day is with Gutlog's own published rubric — flags like natural flavors, seed oils, artificial dyes and sweeteners, each with an evidence level — plus whether to buy produce organic (current Dirty Dozen and Clean Fifteen) and a cleaner swap. Use for "is this clean?", "scan this label", "does this have seed oils", "should I buy organic strawberries", "how clean was my week", "find a cleaner granola"; do not use for logging what they ate (log-food) or gut symptoms (gut-check).
---

# Clean check

Gutlog's clean rubric is its own and is public: each flag says **why** and how
strong the evidence is: regulatory (banned or restricted somewhere), strong,
mixed, or preference. Say the evidence level every time: many clean-label
flags are preference, not settled science. Never use another company's
clean-label brand, logo or wording.

## A packaged food

1. Read the barcode or the ingredient list from the photo.
2. Log it if they ate it (log-food, with `barcode` and `label_text`); the
   label becomes the source of its numbers and ingredients.
3. When your server lists the clean ops (a clean score for a product, meal or
   day, organic guidance and swaps), call them and report the verdict (Clean,
   Mostly clean, Not clean), the 0–100 score and each flag with its evidence
   level, then a swap if there is one. If they are not listed yet, read the
   ingredient list yourself and say which ingredients fall in the rubric's
   categories, marked as your reading, not Gutlog's verdict.

## Produce: organic or not

Organic guidance comes from the current year's EWG Dirty Dozen (buy organic
when you can) and Clean Fifteen (conventional is fine), with the year and the
citation. Anything on neither list: neutral.

## A meal, a day, a week

`query_foods` lists what they ate; with `ingredient` (e.g.
`natural-flavors`, `high-fructose-corn-syrup`) it shows how often a flagged
ingredient showed up and in what.

## Rules

- Profiles: Strict, Balanced (default) or Custom; respect the person's choice.
- No health claims: "clean" here means the rubric, not "healthy" or "safe".
