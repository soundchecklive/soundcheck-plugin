---
name: message-crew-gated
description: Message a gig's crew through Soundcheck only after the user approves. Covers a custom text or email to named people, the standard reminder to the whole gig roster, and nudges to people who have not answered an offer. Every send shows who will be told and waits for a yes.
---

# Message crew (gated)

Use when the user asks to text, email or remind crew, nudge people who have not answered, or broadcast a load-in reminder. Every tool here messages real people, so each one asks for confirmation before it sends.

## Pick the tool

- Specific people, your own words: `send_message` with their user ids.
- The standard reminder to everyone on the gig roster: `broadcast_event_reminder`. Declined and standby people are skipped, and draft gigs are refused.
- People who have not answered a shift: `send_reminder` with kind "crew_offer" and the offer id.
- People who have not answered an availability check: `send_reminder` with kind "availability_request".
- The music schedule or details sheet for a gig: `send_gig_documents` (a dry run first, then send with the plan hash the user approved).

## Sequence

1. **Resolve who.** `list_events` and `get_event` for the gig. `preview_event_recipients` shows who a broadcast would reach and on which channel, without sending. For shift crew, `get_crew_offer` lists each person and their answer. Do not invent recipients.
2. **Draft.** Write the message the user would approve: who, what, when. Keep a text message to one to three short lines.
3. **Ask.** Call the tool once without a confirmation_token. It answers with status confirmation_required and a summary that names the people who will be notified. Show that summary.
4. **Send or stop.** On the user's yes, repeat the same call with the confirmation_token. If they edit the draft, ask again with the new text. If they say no, send nothing.
5. **Report.** Say what the tool returned. Do not claim delivery beyond it.

## Hard rules

- Never send without the user's yes to the summary, and never reuse an expired token.
- Set expected_recipient_count from the preview so a changed roster stops the send.
- Do not change gigs, money or staffing as a side effect of messaging.

## Example

"Remind Saturday's FOH and A2s about the 3pm load-in, but only after I approve."

1. `get_event` for Saturday's gig, `get_crew_offer` for the FOH and A2 shifts to get their user ids.
2. Draft: "Load-in Saturday 3pm at the dock. Reply if you're running late."
3. `send_message` without a token, show the summary, and send with the token after the yes.
