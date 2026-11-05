---
name: log-food
description: Log a meal into Gutlog from a photo you can see, a sentence, a barcode or a label, with gram estimates and every ingredient carrying a confidence; then ask about the uncertain ones and fix anything the person corrects ("the protein was more like 45 g", "no cheese", "it was oat milk"). Use for "log this", "log my lunch", "I had two eggs and toast", "scan this barcode", "that was wrong, fix it"; do not use for reading or auditing the diary (review-diary) or for symptoms (gut-check).
---

# Log food

**You** do the looking: the photo is already in front of you. Gutlog matches
what you send against its food database and barcode index, portions it,
stores it with calories, macros, nutrients and ingredients, and flags what
needs a second look. A meal is always stored; logging costs nothing on any
plan.

## 1. Build the draft

One item per distinct food or drink:

- `name`: plain words ("scrambled eggs", "flat white, whole milk").
- `grams` (your estimate) or `portion: { quantity, unit }` ("1 cup", "2 slices").
- `barcode` when you can read one; `label_text` for an ingredient list or
  nutrition panel you can read verbatim. Labels win over estimates.
- `ingredients`, each with a `confidence` from 0 to 1:
  - ≥ 0.85: you can see it or were told (counted, "confirmed").
  - 0.60–0.85: very likely for this dish (counted, "likely").
  - 0.30–0.60: plausible but hidden: cooking oil or butter, sugar, garlic and
    onion in a sauce, cheese under the top layer. **Include these**: they
    matter for calories and for the gut, and Gutlog asks about them.
  - Under 0.30: leave it out.
- `cooking_method` when it changes the numbers (fried, grilled).

Add `meal_type` and `eaten_at` when it was not just now, `client` (chatgpt,
claude, cursor…) and `model`.

## 2. Log and reply

Call `log_meal`. Reply with the response's one-line `message`, then ask its
`questions` (at most three, highest confidence first): "Was there butter in
the eggs?". Do not read out every nutrient; offer the totals if they ask.

If `needs_review` is true, say so in a few words (low confidence, a possible
trigger or allergen, an odd portion) and offer to fix it now.

## 3. Fix it after

Edits always win over estimates and every edit is recorded. Use the meal edit
ops your server lists (update_meal for a portion or a swapped food,
confirm_ingredients and reject_ingredients for the answers to the questions,
add_ingredients, set_nutrients to lock a value they know, delete_meal,
copy_meal for "same as yesterday's breakfast"). If a needed edit op is not
listed yet, tell the person what you would change and log a corrected meal
only if they ask.

When the person says the analysis was right or wrong, call `rate_analysis`
with `rating` up or down and, for a down rating, what was off in their words
as `reason`. Then fix the meal itself with the edit ops.

## Rules

- Never invent a portion the person gave you differently; their words win.
- Food is health data: never paste it into feedback, issues or other tools.
