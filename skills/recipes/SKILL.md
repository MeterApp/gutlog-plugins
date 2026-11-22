---
name: recipes
description: Develop, save and improve the person's own recipes in Gutlog — work a recipe out from what is in their pantry, save it, save each tweak as a new version, record how every cook turned out, compare versions and promote the better one, add notes and photos, and share a recipe by link. Use for "make me a recipe from what I have and save it", "pull up my dal recipe", "less salt next time", "that version was better, make it the standard", "which version is best", "how did I change this", "add this photo to step 3", "share my chili recipe", "save this shared recipe", "what recipes are waiting on me"; do not use for logging what someone ate (log-food) or for the kitchen inventory itself (pantry).
---

# Recipes: saved, versioned, improved

Gutlog keeps the person's recipes and every version of each one. **You** write
and change the recipe; Gutlog stores it, tracks versions, collects how each
cook turned out and says what the ratings point to. Nothing is analyzed on
Gutlog's side and recipe ops cost nothing on any plan.

How versions work: the first save is version 1, the **current** one (the
standard). Every change is a new version, a **candidate**, until the person
**promotes** it (it becomes current) or **dismisses** it (the current one
stays). Older versions are kept and can be promoted again, which is also how
to go back.

## Develop a recipe from the pantry and save it

1. Call `get_raw_ingredients` (what is on hand, with amounts) and
   `list_expiring` (what to use first). Ask what they feel like if they have
   not said.
2. Work the recipe out with them in chat. Check `get_recipe`'s `watch` list on
   similar saved recipes, or simply ask, before using something they avoid.
3. When they are happy, call `create_recipe`: the title, every ingredient
   (`name`, `quantity`, `unit`, and `grams` when you can estimate them, plus
   `note` such as "finely diced", `optional`, `group` such as "For the sauce")
   and the steps in order (sentences, or `{ text, minutes, tip }`), with
   servings, times and tags.
4. Tell them it is saved and what was missing from the pantry; offer
   `add_shopping_items` for those.

"What can I make tonight?" with recipes already saved: `list_recipes { pantry:
true }` puts the most cookable first and says what each is missing.

## Change a saved recipe

1. Always `get_recipe` first, so the change starts from what is saved, not
   from memory.
2. Call `save_recipe_version` with `change_summary` (what changed and why, in
   a sentence) and only the parts that changed. `ingredients` and `steps` are
   whole lists, not patches; anything you leave out is copied from the version
   it is based on.
3. It is saved as a candidate. Say so: "Saved as version 3. Version 2 is still
   your standard until you have tried this one." Use `promote: true` only when
   they say the change is a plain correction.

Titles, tags and archiving are `update_recipe`. Never call `create_recipe`
again for a recipe that exists.

## After they cook it

Call `record_recipe_cook` for the version that was actually cooked, with their
`rating` (1 to 5; ask, never guess), `would_make_again` and their `notes` on
what worked and what to change. If they have not said how it went yet, record
the cook without a rating and add it later with `cook_id`.

- The result carries the recipe in the shape `log_meal` takes (its
  "log_meal_recipe" field): pass that as `recipe` to put their serving in the
  diary, with `servings_eaten` set.
- Take what was used out of the pantry with `consume_pantry_items`.
- When the result has a `suggestion`, put it to them as a question.

## Decide which version is better

- `compare_recipe_versions` shows what differs between two versions and how
  each was rated, with a `suggestion` (promote, keep_current, cook_first,
  cook_again) and its reason.
- When they say the new one is better: `promote_recipe_version`.
- When they prefer the old one: `dismiss_recipe_version` with their reason.
- Only on their word. Ratings suggest; the person decides.

`get_recipe_nudges` lists what is waiting across all their recipes (versions
with a verdict due, cooks with no rating, candidates never cooked), each with
a question to ask. Call it when a conversation turns to cooking and raise one
or two, not the whole list. A weekly reminder email exists on Plus
(`set_recipe_reminders`); turn it on only if they ask.

## Notes and photos (optional)

- Notes in their words: `add_recipe_note` on the recipe, on a version, or on
  one step (`version` and `step`).
- Photos attach to the recipe (cover and gallery), a version, or a step:
  - An image with a public https address, including one you generated and
    hosted: `add_recipe_image { image: { url } }`. Set `generated: true` for
    anything an image model made.
  - If you hold the bytes (a coding agent, the CLI, the API):
    `add_recipe_image { image: { data, media_type } }`.
  - In a chat where the person attached a photo, or you made an image that
    has no public address, you cannot pass the bytes to a tool. Call
    `create_recipe_upload_link` and give them the link: they open it, pick the
    photos, and it is done. Then `get_recipe` to confirm what arrived.

## Share

- `share_recipe` returns a link anyone can open, with no account, plus a
  Markdown address. The page always shows the current version, so it improves
  as they promote better ones. Ratings, cooks and candidates are never shown;
  their notes only with `include_notes: true`. `unshare_recipe` turns it off.
- Given someone else's link: `get_shared_recipe` reads it, and
  `import_shared_recipe` saves a copy they can then change with
  `save_recipe_version`.

## Rules

- Never promote, dismiss, share or delete on your own judgment.
- `watch` lists ingredients on the person's own trigger or allergen list.
  Mention them plainly; it is their list, not a diagnosis.
- Never paste a recipe's contents into issues or public feedback;
  `report_issue` takes a `request_id`.
