---
name: gut-check
description: Track how the person's gut feels in Gutlog and look for foods and exposures that line up with symptoms — log a check-in (bloating, pain, gas, reflux, stool form, or just "fine"), look back at what was eaten before a bad day, and read suspected triggers as hypotheses to test, never diagnoses. Use for "my stomach hurts", "I'm bloated", "log that I feel fine", "what's been upsetting my stomach", "is it the dairy?", "how's my tummy score"; do not use for starting an elimination program (elimination-diet) or for logging meals (log-food).
---

# Gut check

Gut insights are **hypotheses to test, not diagnoses**. Never tell someone a
food is their cause or give medical advice. Red flags (blood in stool,
unintended weight loss, symptoms that wake them at night, fever, new symptoms
after 50, a family history of colon cancer, celiac or IBD) mean: see a doctor
first, before any analysis or diet change.

## Log a check-in

Ask briefly how they feel: each symptom 0–10 (bloating, abdominal pain, gas,
nausea, reflux, urgency), stool form on the Bristol scale 1–7 if they offer
it, energy, or simply "fine". **"Fine" is a valid, useful log**: trigger
analysis needs good days as much as bad ones. Log it with the symptom op your
server lists (log_symptoms). If it is not listed yet, say so and keep the
details for when it is.

A good rhythm: a check-in 2–4 hours after a meal and one in the evening.
`audit_diary` lists days with meals but no symptom log
(`check: missing_symptoms`); offer to fill the last few.

## Look back at a bad day

1. `get_diary` for that day and the day before (`include_symptoms: true`).
2. Name what they ate in the hours before, with the ingredients Gutlog
   counted. Possible-tier ingredients nobody confirmed are not counted: ask
   about the ones that matter (garlic, onion, dairy, wheat, beans, sugar
   alcohols) and confirm or reject them.
3. `query_foods` with `ingredient` (e.g. `garlic`, `milk`) shows how often
   they had it over the last weeks, to compare with how often they had
   symptoms.

## Suspected triggers

When your server lists the trigger ops (get_triggers, get_exposures), read
them as Gutlog states them: each exposure with an effect size, a confidence
and how many exposures it is based on, or "not enough data yet". Say exactly
that. A suspected trigger is something to test with a structured
reintroduction (elimination-diet), not a food to cut forever.

## Rules

- Symptoms are health data: never paste them into feedback, issues or other
  tools.
- Recommend a registered dietitian for anything beyond tracking.
