---
name: pantry
description: Keep the household's kitchen inventory in Gutlog — what is in the fridge, freezer, cupboards and spice rack, where, and when it expires — from a fridge or pantry photo you read, a receipt, barcodes or a sentence; then say what to use first, what to freeze, what raw ingredients there are to cook with, and what to buy. Use for "here's my fridge", "I just went shopping", "what's expiring", "what should I freeze", "what can I make with what I have", "add milk to the list", "we used the last eggs"; do not use for logging what someone ate (gutlog) or for setup (gutlog-setup).
---

# Pantry: the kitchen inventory

Gutlog keeps the household's food on hand: each item with what it is, how
much, where it is, its state and an expiry date. Shelf life comes from USDA
FoodKeeper data for where the item is stored and its state; nothing is
analyzed on Gutlog's side and pantry ops cost nothing on any plan. **You** do
the looking and the reasoning: read the photo or the receipt, then call the
ops with structured items. Every write takes a list, so one call records a
whole shopping trip; each entry succeeds or fails on its own.

## Inventory a fridge, freezer or cupboard from a photo

1. Call `list_locations` to reuse the household's own names (Kitchen fridge ›
   Door, Garage freezer, Spice rack…).
2. List what you can see, one item per food: a plain `name` ("whole milk",
   "chicken thighs", "baby spinach"), `quantity` and `unit` when you can tell
   (1 gal, 2 lb, 6 each, 1 jar), and any printed date you can read with its
   kind (`use_by`, `best_by`, `sell_by`). Say `opened` when it clearly is.
3. Call `add_pantry_items` with `location` set to where the photo was taken
   (a path like "Kitchen fridge › Door") and `source: "photo"`.
4. Reply with a short summary: how many items, anything that expires within
   three days, any `warning` (e.g. eggs on a shelf instead of the fridge), and
   the `unresolved` names. For those, ask what they are or fix them later with
   `update_pantry_items { ingredient_id }`.

A receipt is the same with `source: "receipt"`, `price` and `store`, and the
location left out (Gutlog puts each item where FoodKeeper keeps it: milk in
the fridge, rice in the pantry, bananas on the counter). Barcodes go in `gtin`.

## Keep it current

- Eaten or cooked with: `consume_pantry_items` by `item_id`, or by
  `ingredient_id` or `name` (the soonest-expiring is used first), with a
  `quantity` in any unit of the same kind (200 g from a 1 lb pack). When it
  went into a meal you logged with `log_meal`, pass its `meal_id`.
- Opened, cooked, recounted, a date read later: `update_pantry_items`.
- Into the freezer or out of it: `move_pantry_items` (it becomes frozen or
  thawed and its date moves).
- Thrown out: `discard_pantry_items` with a `reason` (expired, spoiled,
  freezer_burn, didnt_like, too_much, other).

## Answer questions

- "What's expiring?" `list_expiring` gives each item's `days_left` and a
  suggested `action` (`use`, `freeze` or `toss`) with a why. Never call food
  unsafe that the response only says is past its best.
- "What should I freeze?" `freeze_suggestions`, then `move_pantry_items` to a
  freezer for the ones the person agrees to.
- "What can I make?" `get_raw_ingredients` (deduplicated ingredients on hand
  with amounts and the soonest expiry) plus `list_expiring`: plan with what
  expires first. You suggest the recipes; Gutlog only knows what is there.
- "How long does X keep?" `get_shelf_life` by name.
- "How much did we waste?" `pantry_summary`.

## Shopping

- Staples: `set_par_levels` ("always 12 eggs, a gallon of milk").
- `get_shopping_list` returns added items, staples below par and things used
  up lately. Add with `add_shopping_items`; after the trip,
  `check_off_shopping_items` by `key` puts bought items into the pantry.

## Rules

- The pantry belongs to the household's organization: everyone in it sees and
  changes the same items.
- Dates are estimates from public guidance, not a safety guarantee. When in
  doubt, say "check it" and let the person decide.
- Never paste pantry contents into issues or public feedback; `report_issue`
  takes a `request_id`.
