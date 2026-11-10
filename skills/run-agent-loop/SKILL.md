---
name: run-agent-loop
description: Run Soundcheck's end-to-end agent loop. Ingest gig paperwork, staff the crew, then send one approved message, all in one organization. Use for a full demo or when the user asks to take a gig from paperwork to staffed.
---

# Run the agent loop

The loop: ingest the gig's paperwork, staff the crew, then make one write the user approves. It composes the other skills and stays in one organization throughout.

## Sequence

1. **Identity.** `get_me` lists the user's organizations. Name the one you will use and do not switch.
2. **Ingest.** Follow the ingest-a-gig skill. If there is no file, pasted text works too. Never invent extracted fields.
3. **Plan the day.** If the gig has no times yet, follow the build-run-of-show skill.
4. **Staff.** Follow the staff-a-live-event skill: shifts on the Staffing board, people attached, reviewed.
5. **One approved write.** Send the shifts (staff-a-live-event, step 6) or a crew message (message-crew-gated). Stop at the confirmation summary until the user says yes.
6. **Receipt.** Report what was approved and what the tools returned. Settle money only if asked (settle-a-gig).

## Success criteria

- The user saw a confirmation summary before anything reached a real person.
- One organization throughout.
- No invented crew, times or money.

## Hard rules

- Prefer a few well-chosen tool calls over trying every tool.
- Never skip the confirmation step on a gated write.
- Do not archive or delete gigs.

## Example

"Use Soundcheck to ingest this advance, staff Saturday's gig, and message FOH about load-in, but only after I approve the message."
