---
name: message-crew-gated
description: When to use — send a crew message, invitation follow-up, or event reminder through Soundcheck MCP only after the human approves the confirmation gate (Confirm Card style).
---

# Message crew (gated)

Use when a human asks you to text/email/DM crew, nudge no-shows, or broadcast a load-in reminder for a Soundcheck gig. Member OAuth required. This skill is the **write half** of the agent loop after staffing.

## Prerequisites

1. Soundcheck MCP connected with member OAuth.
2. Know the org (`get_me` / `list_organizations`) and the target event (`list_events` / `get_event`).
3. Prefer existing staffing context (who is INVITED / STANDBY / CONFIRMED) from call lists or event members — do not invent recipients.

## Tools (gated)

These return `confirmation_required` and do **nothing** on the first call:

- `send_message`
- `broadcast_event_reminder`
- `invite_org_member` (when messaging is really an invite)

## Sequence

1. **Resolve** — org + event + recipients (or broadcast scope). State channel if known (SMS / email / in-app).
2. **Draft** — Build the message body the human would approve. Keep it 1–3 short lines for SMS; longer ok for email. Include who / what / when.
3. **Gate (pending Confirm Card)** — Call the write tool **once without** `confirmation_token`. Surface the returned `summary` as a Confirm Card: title (verb + object), risk chip, helper, diff rows (who / what / when / channel), safe preview.
4. **Human decision**
   - Approve → resend the **same** tool call with `confirmation_token`.
   - Reject / edit → do not send the token; revise draft and gate again if they still want a send.
5. **Receipt** — After success, report approved/rejected with timestamp. Do not claim delivery beyond what the tool returns.

## Hard rules

- Never invent or reuse expired confirmation tokens.
- Never send without an explicit human confirm after showing the gate summary.
- Do not archive events, settle, or change money as a side effect of messaging.
- Do not name competing live-event products.
- Prefer `broadcast_event_reminder` for whole-crew load-in nudges; `send_message` for targeted threads.

## Example

Human: "Remind Saturday's FOH and A2s about 3pm load-in — only after I approve."

1. Resolve Saturday event + FOH/A2 recipients
2. Draft short load-in reminder
3. `send_message` without token → show Confirm Card summary
4. On approve → same call with token → receipt
