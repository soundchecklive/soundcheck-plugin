# Soundcheck for Cursor

Cursor plugin that connects Agent to **Soundcheck** — live-event staffing for gigs, crew, setlists, call lists, and document ingest. Package skills plus a URL-only MCP member server. Sign in with OAuth. No API keys.

## Install

**Cursor Marketplace (when listed)** — install **Soundcheck** from the Cursor Marketplace, then connect the MCP server with OAuth when prompted.

**Local test (before / without Marketplace listing)**

```bash
mkdir -p ~/.cursor/plugins/local
ln -s /path/to/this-repo ~/.cursor/plugins/local/soundcheck
```

Reload the window (`Developer: Reload Window`), then open **Customize** and confirm the plugin, skills, and MCP server appear.

## Connect (OAuth, no API keys)

This plugin ships a Streamable HTTP MCP config. Cursor infers transport from `url`. Authentication is OAuth 2.0 (RFC 9728 Protected Resource Metadata) — you do not paste tokens or Bearer headers.

Documented member connect URL (used in `mcp.json`):

`https://mcp.soundchecklive.io/mcp`

Unauthenticated discovery (not packaged in this plugin; paste only when you want the public tools):

`https://api.soundchecklive.io/public/mcp`

See [MCP server docs](https://docs.soundchecklive.io/integrations/mcp-server) and [MCP discovery](https://docs.soundchecklive.io/integrations/mcp-discovery).

## What you get

| Piece | Role |
| --- | --- |
| MCP `soundcheck` | Member tools for events, crew, setlists, ingest, messaging, imports |
| Skill `staff-a-live-event` | Create and staff a gig end-to-end |
| Skill `ingest-a-gig` | Batch → add docs → review → gated commit |
| Skill `clone-workspace` | Export → preview UEF → gated import into an org you own |

Consequential writes (`invite_org_member`, `settle_event`, `send_message`, `broadcast_event_reminder`, ingestion/import commits, and related gated tools) return `confirmation_required`. The agent should show you the summary and only resend with the token after you confirm.

## Usage examples

**Staff a Friday show**

> Staff Friday's show at the Blue Note — FOH, two A2s, and our usual opener set.

Agent runs `staff-a-live-event`: `get_me` / `list_organizations` → `create_event` → link or create venue → call list + `apply_calllist_to_event` → setlist → checklist.

**Ingest an advance PDF**

> Ingest this advance PDF for Saturday's wedding and pull crew and load-in into the gig.

Agent runs `ingest-a-gig`: `create_ingestion_batch` → upload/add text → review proposals → gated commit only after you approve. If analysis fails, it reports that instead of inventing fields.

**Clone last tour into a new org**

> Clone last tour's workspace into our new production-company org.

Agent runs `clone-workspace`: `export_workspace` on the source → `preview_uef_import` on the dest with commit off → `import_uef` only after you confirm. Destination must be an org you own.

## Links

- Product: [soundchecklive.io](https://soundchecklive.io)
- MCP docs: [docs.soundchecklive.io/integrations/mcp-server](https://docs.soundchecklive.io/integrations/mcp-server)
- Discovery: [docs.soundchecklive.io/integrations/mcp-discovery](https://docs.soundchecklive.io/integrations/mcp-discovery)
- AI info: [docs.soundchecklive.io/ai-info](https://docs.soundchecklive.io/ai-info)
- Privacy: [soundchecklive.io/privacy](https://soundchecklive.io/privacy)

## License

MIT © Soundcheck Live, Inc.
