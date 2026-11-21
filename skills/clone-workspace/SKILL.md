---
name: clone-workspace
description: Copy one Soundcheck organization's data (crew, clients, venues, inventory, songs, setlists, gigs with their shifts, leads) into another organization the user manages. Export, preview on the destination, and import only after the user approves.
---

# Clone a workspace

Use when the user wants to copy a sandbox, last tour's organization or a sample workspace into another organization. The destination must be one the user owns or manages.

## Sequence

1. **Pick both organizations.** `get_me` lists the user's organizations with their roles. Confirm the source and the destination by name. Never import into an organization the user did not name.
2. **Say what stays behind.** The export leaves out lead budgets, inventory replacement costs, crew performance fees, files, branding and integration tokens. Tell the user before starting.
3. **Export.** `export_workspace` on the source returns one Soundcheck import file. Keep it for the next steps.
4. **Preview.** `preview_import_file` with that document on the destination writes nothing. Show the new and updated counts and any errors.
5. **Import, with approval.** `import_file` writes the records and may email organization invitations to team members in the file, so it asks for confirmation. Show the summary, wait for the user's yes, then repeat the call with the confirmation_token.
6. **Check.** Spot-check with `list_org_members`, `list_venues` and `list_events` on the destination, and report what landed.

## Hard rules

- Always preview before importing.
- Money, files, branding and secrets do not transfer; say so up front.

## Example

"Clone last tour's org into our new production company."

1. `get_me` to find both organizations.
2. `export_workspace` on last tour's organization.
3. `preview_import_file` on the new company and show the counts.
4. After the user's yes, `import_file` with confirmation, then spot-check.
