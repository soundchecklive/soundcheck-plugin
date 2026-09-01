---
name: staff-a-live-event
description: When to use — staff or set up a live event (gig) in Soundcheck as a signed-in member: create the event, link a venue, apply a call list, build a setlist, and work the checklist. Prefer OAuth member tools over public intake.
---

# Staff a live event

Use this skill when a human asks you to create, staff, or prep a gig in Soundcheck (Friday show, wedding, corporate booking, tour date). You are a third-party agent acting as the signed-in user. Connect with member OAuth first; do not invent API keys.

## Prerequisites

1. Confirm the Soundcheck MCP server is connected via OAuth (member path).
2. Call `get_me`, then `list_organizations`, and confirm which org is active for this work.
3. Prefer member tools. Use public intake (`request_booking` / `request_sponsorship`) only as a last resort when the human is **not** a member and only needs anonymous lead intake — never for staffing their own org.

## Sequence

Work in this order unless the human already has some steps done:

1. **Identity** — `get_me` → `list_organizations`. State which org you will use.
2. **Create the event** — `create_event` with name, date(s), and any known details. Pass `member_ids` only when the human asks to override the default (authenticated user is added by default).
3. **Venue** — `list_venues` / `get_venue`. If it exists, `link_venue_to_event`. If not, `create_venue` then link.
4. **Call list** — Reuse with `list_calllists` / `get_calllist`, or `create_calllist` / `update_calllist`. Then `apply_calllist_to_event` (primaries → `INVITED`, backups → `STANDBY`). Re-apply adds 0 new rows.
5. **Setlist** — `create_song` as needed, then `create_setlist` / `update_setlist` for the event.
6. **Checklist** — `get_event_checklist`, then `update_checklist_task` (always pass `checklist_version` from the latest read).

Optional follow-ups after the core setup: availability requests, mailbox accepts/declines, messaging, or settlement — only when asked.

## Confirmation-gated writes

These tools return `confirmation_required` (with `confirmation_token`, `summary`, `expires_in_seconds`) and do **nothing** on the first call:

- `invite_org_member`
- `settle_event` (also `complete_event`, `reopen_event`)
- `send_message`
- `broadcast_event_reminder`

**Protocol:**

1. Call once **without** a confirmation token.
2. Show the returned `summary` to the human.
3. Resend the **same** call with the token **only** if they confirm.
4. Do not invent or reuse expired tokens.

## Hard rules

- **Never** call `archive_event` unless the human explicitly asks to archive. It is still ungated.
- Do not settle, message crew, broadcast reminders, or invite org members without human confirmation via the gate above.
- Do not name competing live-event products.
- Do not claim data you did not read from tools.

## Example

Human: "Staff Friday's show at the Blue Note — FOH, two A2s, and our usual opener set."

1. `get_me` / `list_organizations`
2. `create_event` for Friday
3. Find or create Blue Note → `link_venue_to_event`
4. Build or reuse a call list with FOH + A2 seats → `apply_calllist_to_event`
5. Add opener songs → setlist tools
6. Walk open checklist items with `get_event_checklist` / `update_checklist_task`
