---
name: clone-workspace
description: When to use — copy one Soundcheck organization's catalogs (crew, venues, songs, events, etc.) into another org the user owns via export_workspace → preview_uef_import → import_uef, with human confirmation before commit.
---

# Clone a workspace

Use this skill when a human wants to clone a sandbox, last tour's org, or golden fixture into a destination organization. Member OAuth only. Destination must be an org the user **owns** (or can administer for UEF import). Public MCP cannot clone workspaces.

## Prerequisites

1. Soundcheck MCP connected with member OAuth.
2. Identify **source** and **destination** org ids via `list_organizations` / `get_organization`.
3. Confirm the human owns (or may import into) the destination. Do not import into a production org they did not explicitly name.
4. Remind them what does **not** transfer: money/ledger, file blobs, branding, forms, integration tokens, invitation accept state, tours/run-of-show links, and similar non-export fields.

## Sequence

1. **Export source** — `export_workspace` on the source organization. Keep the returned UEF / workspace payload for the next steps.
2. **Dry-run on dest** — `preview_uef_import` on the destination with `committed: false` (preview / dry run). Show counts, warnings, and what would be created vs matched.
3. **Human gate** — Summarize the preview. Do **not** call `import_uef` until they explicitly confirm the destination and the payload.
4. **Commit** — `import_uef` is confirmation-gated:
   - Call once without a token → show `summary`
   - Resend with `confirmation_token` only after they confirm
5. **Verify** — Spot-check with reads (`list_org_members`, `list_venues`, `list_events`, etc.) and report what landed vs what stayed out of the dump.

## Hard rules

- Destination must be an org the user owns / is allowed to import into — never assume.
- Always preview with `committed: false` (or equivalent dry-run) before `import_uef`.
- Money, files, branding, and secrets stay out of the clone; say so up front.
- Do not use public `request_booking` / `request_sponsorship` for workspace clone.
- Do not name competing live-event products.

## Example

Human: "Clone last tour's org into our new production company org."

1. `list_organizations` — pick last-tour source and the new company dest (owned)
2. `export_workspace` on source
3. `preview_uef_import` on dest (`committed: false`) — show catalog counts and warnings
4. After explicit confirm → `import_uef` (gate → confirm → token)
5. Summarize restored catalogs and exclusions
