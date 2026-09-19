---
name: run-agent-loop
description: When to use — run Soundcheck’s flagship agent loop end-to-end: ingest gig paperwork → staff crew → gated confirm on a write (message/invite). Prefer this over ad-hoc tool spam for Muse/ChatGPT/Claude demos.
---

# Run the agent loop (ingest → staff → confirm)

Flagship path for third-party agents (Muse, ChatGPT, Claude, Cursor). Member OAuth. Goal: one airtight loop humans can trust.

## Loop

```
ingest gig → staff crew → gated confirm (message / invite) → optional settle preview
```

## Skills to compose

1. **ingest-a-gig** — paperwork → proposals → gated commit only after approve
2. **staff-a-live-event** — event, venue, call list, setlist, checklist
3. **message-crew-gated** — Confirm Card on `send_message` / `broadcast_event_reminder`
4. Optional: **settle-a-gig** — money preview / gated settle only if asked

## Sequence (demo-safe)

1. **Cold identity** — `get_me` → `list_organizations`. Name the org (id + name). Do not use another tenant.
2. **Ingest** — Follow `ingest-a-gig`. If no file, accept pasted advance text via `add_ingestion_text`. Never invent extract fields.
3. **Staff** — Follow `staff-a-live-event` for missing venue / call list / setlist pieces. Prefer reuse over create.
4. **Confirm write** — Propose one irreversible-feeling action (crew message or invite). Run `message-crew-gated`. Stop at the gate until the human approves once.
5. **Receipt** — Show approved/rejected state. Optional settle preview only if they ask.

## Success criteria

- Human saw at least one Confirm Card style gate before a write landed
- Same org throughout
- No invented crew, times, or money
- Public-safe narrative (no secrets in demos)

## Hard rules

- Prefer fewer tool calls with clear describes over dumping the whole tool list
- Never skip the confirmation token protocol on gated writes
- Do not archive events
- Do not claim Muse/OpenAI listing status in the skill body

## Example prompt

"Use Soundcheck to ingest this advance, staff Saturday’s gig from our roster, and message FOH about load-in — only after I approve the message."
