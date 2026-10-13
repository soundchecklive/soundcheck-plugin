<p align="center">
  <img src="assets/logo.png" alt="Soundcheck" width="128" height="128" />
</p>

# Soundcheck for Cursor

[![smithery badge](https://smithery.ai/badge/steven-pd2q/soundcheck)](https://smithery.ai/servers/steven-pd2q/soundcheck)

Cursor plugin that connects Agent to **Soundcheck** — live-event staffing for gigs, crew, setlists, call lists, and document ingest. **Headline: the Soundcheck member MCP** (`https://mcp.soundchecklive.io/mcp`) — sign in with OAuth, no API keys, and run your org's gigs, crew, setlists, and settlement from your agent. A secondary no-login public MCP is included only as a fallback for reviewers and discovery.

Smithery: https://smithery.ai/servers/steven-pd2q/soundcheck

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

Secondary — public no-login fallback for reviewers and discovery (packaged as `soundcheck-public`):

`https://mcp.soundchecklive.io/public/mcp`

## Endpoints

| Server | URL | Auth |
| --- | --- | --- |
| `soundcheck` (member, primary) | `https://mcp.soundchecklive.io/mcp` | OAuth 2.0 via Soundcheck sign-in (`accounts.soundchecklive.io`). No API keys. |
| `soundcheck-public` (secondary, no-login fallback) | `https://mcp.soundchecklive.io/public/mcp` | None (no login) |

Domains: `soundchecklive.io`, `mcp.soundchecklive.io`, `accounts.soundchecklive.io`.

**Sample test task — `/mcp` (member):** sign in when prompted, then ask: *"List my Soundcheck organizations and show upcoming events for the first one."* (expects `get_me` → `list_organizations` → `list_events`).

**Sample test task — `/public/mcp` (no login):** ask: *"What would a 150-guest wedding in Austin with a DJ and sound tech cost?"* (expects `quote_event` with a SYNTHETIC illustrative range).

## Reviewer notes

A reviewer test account is provided privately in the marketplace submission form so reviewers can sign into the member MCP (`soundcheck`, `/mcp`) and exercise the full experience. `soundcheck-public` is only a no-login fallback. No credentials are stored in this repository. Gated writes (messages, invites, settle, imports) return `confirmation_required` and do nothing until confirmed.

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


## Skills

- `clone-workspace` — clone catalogs between orgs you own
- `ingest-a-gig` — paperwork → gated commit
- `staff-a-live-event` — event, venue, call list, setlist, checklist
- `message-crew-gated` — Confirm Card on crew message / reminder
- `settle-a-gig` — gated settle / complete / reopen
- `run-agent-loop` — ingest → staff → confirm (demo path)
- `buy-platform-plan` — platform SaaS purchase / pricing handoff


## Claude plugins (directory)

This repository also packages plugins for Anthropic’s Claude Plugins directory:

| Plugin path | Product | MCP | Auth |
| --- | --- | --- | --- |
| repo root (`.claude-plugin/`) | **Soundcheck** member staffing / ops | `https://mcp.soundchecklive.io/mcp` | OAuth |
| `instant-quotes/` | **Soundcheck Instant Quotes** | `https://mcp.soundchecklive.io/public/mcp` | None |

Instant Quotes estimates are **SYNTHETIC / illustrative**, not binding. Member Soundcheck is signed-in org ops — a different product.
