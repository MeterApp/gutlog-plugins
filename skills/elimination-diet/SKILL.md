---
name: elimination-diet
description: Run a structured elimination and reintroduction in Gutlog — the three-phase low-FODMAP protocol used in IBS care or a single-food test (dairy, gluten, a suspected trigger) — starting with a safety screen, then swaps for the person's own foods, daily check-ins, one-at-a-time challenges and a personal tolerance list, always alongside a registered dietitian. Use for "should I try low FODMAP", "help me test if it's dairy", "start an elimination diet", "what's today's challenge", "how did the reintroduction go"; do not use for one-off symptom logs (gut-check).
---

# Elimination diet

This is a test, not a cure, and **never** a diagnosis. Every program
recommends working with a registered dietitian, and it is not for everyone.

## Safety screen first (always)

Before suggesting any program, ask:

- Red flags: blood in stool, unintended weight loss, night-time symptoms,
  fever, anemia, new symptoms after age 50, a family history of colon cancer,
  celiac disease or IBD. **Any yes: see a doctor first**; do not start.
- History of an eating disorder, pregnancy or breastfeeding, or under 18:
  **no restrictive protocol**. Offer symptom tracking (gut-check) instead.

When your server lists the program ops (start_program and the phase and
challenge ops), they run this screen and record the answers; pass the
person's answers, never your guesses.

## Low-FODMAP, three phases

1. **Elimination** (2–6 weeks): swap high-FODMAP foods for low-FODMAP ones
   from the person's own diary. `query_foods` (sort `times`) shows what they
   eat most; suggest swaps for those first. Daily check-ins (gut-check) and a
   weekly "adequate relief?" question. **Stop at 6 weeks** if nothing has
   improved, and say so.
2. **Reintroduction**: one FODMAP subgroup at a time (fructans, GOS, lactose,
   excess fructose, sorbitol, mannitol), three days of increasing doses of a
   challenge food, then 2–3 washout days. Log each challenge meal with
   log-food and a symptom check-in after.
3. **Personalization**: back to a broad diet, limiting only the subgroups and
   doses that caused symptoms.

## Single-food test

Remove one food or exposure (dairy, gluten, a suspected trigger) for 2–4
weeks with daily check-ins, then reintroduce it and compare.

## While it runs

- `get_nutrition_summary` over the phase: make sure fiber, calcium and energy
  have not dropped; if they have, say so and suggest the dietitian.
- `audit_diary` keeps the data clean (missing meals, unconfirmed garlic and
  onion matter most here).

## Rules

- Never tell someone to cut a food group permanently.
- Health data stays in Gutlog: never paste it into feedback or other tools.
