---
name: ingest-a-gig
description: When to use — turn gig paperwork (contracts, call sheets, riders, emails, PDFs) into structured Soundcheck event proposals via MCP ingestion, then review and commit only what the human approves.
---

# Ingest a gig

Use this skill when a human wants to pull details from advance docs, contracts, call sheets, riders, or pasted email into a Soundcheck event. You are a third-party agent. Member OAuth is required. Do not invent extracted fields when analysis fails.

## Prerequisites

1. Soundcheck MCP connected with member OAuth.
2. Know the target event id (`list_events` / `get_event`, or create one first with `create_event` if the human asks).
3. Prefer member ingestion tools. Public `request_booking` / `request_sponsorship` are **not** substitutes for file ingest.

## Sequence

1. **Open a batch** — `create_ingestion_batch` on the event.
2. **Add content**
   - Pasted text / email body → `add_ingestion_text`
   - Binary file (PDF, image) → `upload_ingestion_file` (base64) or follow `request_attachment_upload` when the host app stages files
3. **Wait and review**
   - Poll with `get_ingestion_batch` / `list_ingestion_batches`
   - Per file: `get_ingestion_file_review` (proposals + dry-run new vs match)
   - Optional merge: `trigger_ingestion_merge`, then `get_ingestion_batch_review`
4. **Report honestly** — Show schedule, crew, ledger, leads, and event-detail proposals that analysis produced. If a file is attached-only, analysis failed, or proposals are empty, say so. **Do not invent** load-in times, fees, names, or venues.
5. **Commit only after approval**
   - `commit_ingestion_file` or `commit_ingestion_batch` are confirmation-gated
   - Call once without a token → show `summary` → resend with `confirmation_token` only if the human confirms

## Failure and recovery

- Analysis may not produce proposals; files can land attached-only. Report status; do not fabricate data.
- Use `retry_ingestion_file` only when the human wants a retry.
- Use `dismiss_ingestion_file` / `dismiss_ingestion_merge` when they want to drop a bad extract.
- If a re-merge changes proposals, any prior confirmation token is invalid — review again before commit.

## Hard rules

- Nothing writes to the gig until a gated commit succeeds after human confirmation.
- Never invent extracted data to "fill gaps."
- Do not spam public intake tools for paperwork the member already owns.

## Example

Human: "Ingest this advance PDF for Saturday's wedding and pull crew + load-in."

1. Resolve Saturday's event
2. `create_ingestion_batch`
3. `upload_ingestion_file` with the PDF
4. `get_ingestion_file_review` / `get_ingestion_batch_review` — present proposals or report analysis failure
5. On approval, `commit_ingestion_batch` (gate → confirm → token)
