---
name: ingest-a-gig
description: Bring outside gig information into Soundcheck. Covers advance sheets, contracts, riders, email threads and schedule files, plus spreadsheets and public event page links. Preview what was found, review it with the user, then save only what they keep. Never invent fields the analysis did not find.
---

# Ingest a gig

Pick the path from what the user has. Every path previews first, shows the user what was found, and writes only what the user keeps.

- A document about one gig (advance sheet, contract, rider, call sheet, email thread, itinerary, a photo of a schedule): file ingestion, below.
- A spreadsheet of many gigs, leads, gear or people: tabular import, below.
- A public event page link: if the server lists `preview_event_from_url`, use it, review the fields and sections with the user, then `import_event_from_url` with the sections they keep. Otherwise paste the page text into file ingestion.
- A Soundcheck export file: use the clone-workspace skill.

## File ingestion

1. **Find the gig.** Files attach to a gig: find it with `list_events`, or create a draft with `create_event` if the user asks.
2. **Open a batch.** `create_ingestion_batch` on the gig.
3. **Add the files.** In the Soundcheck app with attachments staged, call `request_attachment_upload` once and stop; the app uploads them. Otherwise call `add_ingestion_file` once per file: text for pasted text, or filename plus content_base64 for a PDF, image or spreadsheet (up to 8 MiB).
4. **Wait once.** Analysis runs in the background. Check `list_ingestion_batches` with batch_id one time; if it is not ready, tell the user and check again later instead of polling.
5. **Review with the user.** `list_ingestion_batches` with batch_id and view "review" gives the merged findings across files; `get_ingestion_file_review` gives one file's. Walk the user through the run of show rows, crew, money lines, leads and warnings, and flag rows outside the gig's dates. If a file was only attached, analysis failed, or nothing was found, say so.
6. **Save what they kept.** `commit_ingestion_batch` with the kept rows' document paths as selected and the review's merge_version, or `commit_ingestion_file` for one file. This writes live data and cannot be undone here, so it asks for confirmation: show the summary, wait for a yes, then repeat the call with the confirmation_token.

## Tabular import

`create_import_from_paste` with the CSV text and target type, then `map_import` with no mapping to auto-map (`get_import_field_catalog` lists the fields), then `preview_import`. Show the new, updated and error counts, then `commit_import` after the user's yes. Ask before saving a mapping as a template.

## Hard rules

- Nothing is written to the gig until a confirmed commit succeeds.
- Never fill gaps with invented load-in times, fees, names or venues.
- A new merge changes the proposals: review again before committing.

## Example

"Ingest this advance PDF for Saturday's wedding and pull crew and load-in."

1. `list_events` to find Saturday's gig.
2. `create_ingestion_batch`, then `add_ingestion_file` with the PDF.
3. `list_ingestion_batches` with view "review": present what was found, or report that analysis failed.
4. After the user picks rows, `commit_ingestion_batch` with confirmation.
