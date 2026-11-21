---
name: settle-a-gig
description: Close out a Soundcheck gig's money. Read the ledger and show settlement, mark the gig completed, then settled, or reopen a premature closeout. Each closeout step waits for the user's approval.
---

# Settle a gig

Use when the user asks to close out, complete, settle or reopen a gig, or asks what settlement would look like. This touches money, so read first and ask before every write.

## Sequence

1. **Find the gig.** `list_events` and `get_event`.
2. **Read the money.** `get_event_ledger` lists payables, receivables, income and expenses with what is paid and what remains, and its settlement field says whether the gig is ready to settle (unsettled_count and unsettled_item_ids). Report only the numbers it returns.
   - For a gig with a deal, and if the server lists `get_settlement`, read the deal worksheet too: guarantee, addenda, overtime, what was paid, the balance, and anything that needs review.
3. **Fix open lines if asked.** Log a payment made outside Soundcheck with `record_ledger_payment`; undo one with `reverse_ledger_payment`. For billing and payouts, hand off with `get_soundcheck_link`.
4. **Complete.** A past, active gig is first marked completed with `complete_event`.
5. **Settle.** `settle_event` marks a completed gig settled. Check the ledger's settlement summary first.
6. **Reopen if needed.** `reopen_event` undoes a premature closeout.

Each of `complete_event`, `settle_event` and `reopen_event` asks for confirmation: call once, show the summary, and repeat the call with the confirmation_token only after the user's yes. Afterwards read `get_event` again and report the new status.

## Hard rules

- Do not invent ledger lines, tips, tax or balances.
- When the user only asked what settlement would look like, stop after step 2.
- Do not message crew or change staffing as a side effect.

## Example

"Show me Friday's settlement, then settle it if it looks right."

1. `get_event_ledger` for Friday's gig, and present the balance and open lines.
2. On the user's yes, `complete_event` (if not completed yet), then `settle_event`, each with confirmation.
