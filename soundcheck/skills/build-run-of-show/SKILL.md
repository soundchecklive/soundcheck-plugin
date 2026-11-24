---
name: build-run-of-show
description: Build a gig's run of show in Soundcheck. Set the key times, lay out timed segments and sections, add the setlist, then read it back. Use when the user asks to plan the day, set doors or curfew, lay out the schedule, or add songs to a set.
---

# Build a run of show

The run of show is the gig's day-of plan: key times, then sections and cues, then the songs in each set. Times are in the gig's time zone; anything after midnight still belongs to the same gig day.

1. **Read what is there.** `get_event` for the dates and venue, then `get_event_run_of_show` for the sections, items, cues and its version. If the org has not adopted Run of Show, `get_event_schedule` shows its timeline instead.
2. **Set the key times.** `get_event_key_times` returns crew call, load in, soundcheck, doors, show, curfew and load out, plus a revision. Edit the list and send it all back with `set_event_key_times` and that revision: any key time left out is removed. Sections tied to a key time move with it.
3. **Lay out the segments.**
   - To add a few timed rows, use `create_event_schedule_items` (start_time as RFC3339 with offset).
   - To add sections, reorder, or change cues, edit the document from step 1 and send it whole with `update_event_run_of_show` and expected_version. A 412 means someone else edited it: read it again and reapply the change.
4. **Add the setlist.** Songs must exist in the library: check `list_songs` and add missing ones with `create_song`.
   - If `list_setlists` with event_id returns a setlist whose id is the gig id, the org keeps sets in the run of show: put the song ids on the set's run of show items with `update_event_run_of_show`.
   - Otherwise try `save_setlist` with event_id. If it answers 409 SETLISTS_RETIRED, the org keeps sets in the run of show (a gig with no sets yet shows none in the list): add the set as a run of show item with its song ids through `update_event_run_of_show`.
5. **Read it back.** After editing the run of show document, read it with `get_event_run_of_show`; after editing timeline rows or key times, read `get_event_schedule`. Show the result to the user and ask what to change.

## Watch for

- `update_event_run_of_show` replaces the whole document, so always start from a fresh read. It asks for confirmation: show the user the change and wait for a yes.
- `upsert_event_schedule` replaces the entire timeline and deletes rows you leave out. Use it only when the user wants the whole timeline replaced.
- Key times and run of show edits do not message crew. Changing the gig's date, times or venue with `update_event` does, so treat that as a send.

## Example

"Doors at 7, show at 8, curfew 11, and put our usual opener set in."

1. `get_event_key_times`, then `set_event_key_times` with doors, show and curfew added.
2. `list_setlists` with event_id to see where sets live, then add the songs.
3. Read it back as in step 5 and show the user.
