---
name: staff-a-live-event
description: Staff a Soundcheck gig with crew and message them. Find or create the gig, put shifts on its Staffing board, attach people, review, send crew offers only after the user approves, and follow up. Also covers texting or emailing crew, the standard roster reminder and nudges, each sent only after the user approves. Use when the user asks to staff, crew or fill a gig, send offers, message or remind crew, or chase people who have not answered.
---

# Staff a live event

Shifts (crew offers) are how people get onto a gig. Nothing reaches anyone until step 6, and every send needs the user's yes. You act as the signed-in member.

1. **Pick the organization.** `get_me` lists the user's organizations. Say which one you will use and stay in it.
2. **Find the gig.** `list_events` (match row.event.title), then `get_event` for its dates and roster. If it does not exist, ask whether it should be a draft or live, then `create_event`. To set its venue, find it with `list_venues` (or add it with `save_venue`) and `link_venue_to_event`.
3. **Pick positions.** `list_org_positions` lists the roles this org staffs. Use its names; do not invent roles. If the list is empty, add one with `save_org_position`.
4. **Put shifts on the Staffing board.** Either:
   - From a team template: `get_staffing_plan` with scope "templates" to choose one, then `apply_calllist_to_event`. It makes one draft shift per position slot with the template's people penciled in, and sends nothing.
   - By hand: `save_crew_offer` with title, event_id, org_position_id, a date or days, call times and fee.

   Venue or union stagehand counts are hands, not named people: set them with `save_gig_labor_call`.

5. **Attach people and review.** On a draft shift, the people you pass to `save_crew_offer` replace everyone attached, so read the shift with `get_crew_offer` first and send the full list to keep (offer_id plus recipient_ids, or recipients with each per-person fee), the people already there included. Then `review_staffing_draft` shows every shift with seats, fees, who is attached and any time overlaps. Tell the user about overlaps before sending.
6. **Send, with approval.** Call `send_crew_offer` for each shift. When it answers with status confirmation_required, show the summary (it names the shift and everyone who will be notified), wait for the user's yes, then repeat the same call with the confirmation_token. Never send without that yes.
7. **Follow up.** `get_crew_offer`, or `list_crew_offers` with scope "organization" and the event_id, shows who accepted, declined or has not answered. Nudge the quiet ones with `send_reminder` and kind "crew_offer" (also gated). To ask someone who declined again, call `send_crew_offer` with their id in recipient_ids and the days in reask_dates.

## Message crew

Every tool here messages real people, so each one asks for confirmation before it sends.

- Specific people, your own words: `send_message` with their user ids.
- The standard reminder to everyone on the gig roster: `broadcast_event_reminder`. Declined and standby people are skipped, and draft gigs are refused.
- People who have not answered a shift: `send_reminder` with kind "crew_offer" and the offer id.
- People who have not answered an availability check: `send_reminder` with kind "availability_request".
- The music schedule or details sheet for a gig: `send_gig_documents` (a dry run first, then send with the plan hash the user approved).

1. **Resolve who.** `preview_event_recipients` shows who a broadcast would reach and on which channel, without sending. For shift crew, `get_crew_offer` lists each person and their answer. Do not invent recipients.
2. **Draft.** Write the message the user would approve: who, what, when. Keep a text message to one to three short lines.
3. **Ask.** Call the tool once without a confirmation_token. It answers with status confirmation_required and a summary that names the people who will be notified. Show that summary. Set expected_recipient_count from the preview so a changed roster stops the send.
4. **Send or stop.** On the user's yes, repeat the same call with the confirmation_token. If they edit the draft, ask again with the new text. If they say no, send nothing. Never reuse an expired token.
5. **Report.** Say what the tool returned. Do not claim delivery beyond it, and do not change gigs, money or staffing as a side effect of messaging.

## Watch for

- Crew offers are not held back by a draft gig: a sent shift reaches people even while the gig is hidden from crew. Say so when the gig is a draft.
- Changing the date, call times or pay of a shift that was already sent messages everyone on it. Treat it like a send and get the user's yes.
- For the day-of plan and setlist, use the build-run-of-show skill. For pre-show tasks, `get_event_checklist` and `update_checklist_task`.
- Report only what the tools returned. Do not invent crew, times or fees.

## Example

"Staff Friday's show at the Blue Note: FOH, two A2s."

1. `get_me`, then `list_events` to find Friday's gig.
2. `list_org_positions` to get the FOH and A2 position ids.
3. `save_crew_offer` for a FOH shift (1 seat) and an A2 shift (2 seats), attaching the people the user names.
4. `review_staffing_draft`, then `send_crew_offer` for each shift after the user approves the summary.

"Remind Saturday's FOH and A2s about the 3pm load-in, but only after I approve."

1. `get_event` for Saturday's gig, `get_crew_offer` for the FOH and A2 shifts to get their user ids.
2. Draft: "Load-in Saturday 3pm at the dock. Reply if you're running late."
3. `send_message` without a token, show the summary, and send with the token after the yes.
