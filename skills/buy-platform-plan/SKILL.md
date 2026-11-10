---
name: buy-platform-plan
description: Help someone choose or start a Soundcheck plan (Starter, Pro, Team, or Enterprise). Explain the plans, start a 14-day trial with the user's approval where the tool is available, or send them to the pricing page.
---

# Buy a Soundcheck plan

Use when someone asks to buy Soundcheck, upgrade, or pick a plan. This is the Soundcheck subscription, not a gig's money (that is settle-a-gig).

## Sequence

1. **Clarify the need.** A solo act, a band or a production company maps to Starter, Pro or Team. Enterprise is a conversation with sales.
2. **Use real facts.** Plan prices and limits are on https://soundchecklive.io/pricing. Do not invent prices or discounts.
3. **Start a trial, with approval.** For an organization admin, and if the server lists `start_plan_trial`, it starts a 14-day trial with no card. It creates a real subscription that invoices when the trial ends, and each organization gets one trial, so it asks for confirmation: show the summary and repeat the call with the confirmation_token only after the user's yes. If the tool is missing or refuses, send the pricing page instead.
4. **Billing.** For card changes, invoices and payouts, hand off with `get_soundcheck_link`. Once trials are available (if the server lists `start_plan_trial`), `get_organization` with view "subscription_invoice" also shows an open invoice with its payment link.

## Hard rules

- Never collect card numbers in chat.
- Do not invent promo codes, tax or invoices.

## Example

"Get my band on Soundcheck Pro."

1. Confirm Pro fits the band.
2. If the server lists `start_plan_trial`, start a Pro trial with the user's approval; otherwise share the pricing link.
