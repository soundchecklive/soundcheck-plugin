---
name: buy-platform-plan
description: When to use — help a human discover or purchase a Soundcheck platform plan (Starter/Pro/Team) via agentic commerce / Stripe once ACS checkout is enabled; until then, send them to pricing with clear plan diffs.
---

# Buy a Soundcheck platform plan

Use when someone asks to buy Soundcheck, upgrade seats, or pick Starter/Pro/Team through an agent. This is **platform SaaS purchase**, not gig settle fees.

## Current state (agents)

- Public pricing: https://soundchecklive.io/pricing
- Stripe ACS catalog may list plans with `disable_checkout=true` until tax registration clears — if checkout is disabled, **do not** invent a payment path. Link pricing and explain trials.
- Muse connector may still be listed as non-payments until SPT/ACS is live.

## Sequence

1. **Clarify need** — solo / band / production company → map to Starter / Pro / Team (Enterprise = contact sales).
2. **Read pricing page facts** — member limits, admin seats, trial. Do not invent prices if tools lack them; use the public pricing URL.
3. **If ACS/SPT checkout tools exist and checkout is enabled** — follow those tools’ confirmation protocol. Never paste card numbers.
4. **If checkout disabled** — send https://soundchecklive.io/pricing (or Book a demo for Enterprise). Say tax/agent checkout is still enabling.

## Hard rules

- Never invent promo codes, tax, or invoices.
- Never collect raw card data in chat.
- Do not confuse platform purchase with `settle_event`.

## Example

Human: "Buy Soundcheck Pro for my band through the agent."

1. Confirm Pro fits (≤25 members)
2. If agent checkout live → gated purchase flow
3. Else → pricing URL + 14-day trial note
