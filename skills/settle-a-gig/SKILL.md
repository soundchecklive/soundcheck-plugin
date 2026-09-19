---
name: settle-a-gig
description: When to use — preview and settle (or reopen/complete) a Soundcheck event’s money/closeout via gated MCP tools; never settle without human confirmation.
---

# Settle a gig

Use when a human asks to close out, settle, complete, or reopen a Soundcheck gig’s finances. Member OAuth required. Money risk — treat every write as Confirm Card–gated.

## Prerequisites

1. Soundcheck MCP connected with member OAuth.
2. Resolve org + event (`get_me`, `list_organizations`, `list_events` / `get_event`).
3. Read current settle/ledger state with available read tools before proposing a write.

## Gated tools

- `settle_event`
- `complete_event`
- `reopen_event`

Protocol: call once without token → show `summary` → resend with `confirmation_token` only after explicit confirm.

## Sequence

1. **Read** — Pull event + any settle/ledger preview the tools expose. Report numbers you actually read; do not invent fees or balances.
2. **Propose** — State what settle/complete/reopen would change (who gets paid, totals, status flips).
3. **Gate** — Call the appropriate tool without token; present Confirm Card (risk = money).
4. **Confirm** — Only on approve, resend with token.
5. **Verify** — Re-read event/settle status and report the new state.

## Hard rules

- Never settle, complete, or reopen without the human confirming the gate summary.
- Never invent ledger lines, tip-outs, or tax amounts.
- Prefer settle preview language when the human only asked “what would settlement look like.”
- Do not message crew or archive as a side effect unless separately asked (and gated).

## Example

Human: "Preview settlement for Friday’s wedding, then settle if I approve."

1. Resolve Friday event + read settle state
2. Present preview from tools
3. `settle_event` without token → Confirm Card
4. On approve → token → verify
