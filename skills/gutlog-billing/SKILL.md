---
name: gutlog-billing
description: Answer what Gutlog costs and what the person uses — plans and prices with no sign-in, today's photo analyses against the daily allowance, history, what Plus adds, and a Stripe Checkout link for a human to upgrade. Use for "how much is Gutlog", "why was my photo analysis refused", "what does Plus include", "upgrade me to Plus", "cancel my plan"; do not use for logging food (gutlog).
---

# Gutlog plans, usage and upgrades

Logging a meal you analyzed yourself (`log_meal` from a chat assistant), barcode
scans and every edit are free and unlimited on every plan. What is metered is
**analyses**: a photo or text analysis on Gutlog's own model (web app, CLI,
API, email photos), counted per UTC day.

1. **Prices** (no credential): `get_pricing` returns Free and Plus with monthly
   and yearly prices, analyses per day, history days, features and a
   checkout link.
2. **Usage:** `get_account` returns the plan, today's analyses used and left
   (and when they reset), and what the next plan adds with a checkout link.
   `get_billing` adds the subscription status and whether a card is saved.
3. **Upgrade:** `create_checkout_link` with `{ "plan": "plus", "interval":
   "month" }` (or `"year"`) returns a Stripe Checkout URL for a human; nothing
   is charged until they pay. `subscribe` charges the saved card only with
   `confirm: true` after the human approved the price.
4. **Invoices, card, cancel:** `get_billing_link` (Stripe portal, for a
   human), `cancel_subscription` / `resume_subscription` with `confirm: true`
   only when the human asked.

Never quote a price from memory: read `get_pricing`.
