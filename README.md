<p align="center">
  <img src="assets/logo.png" alt="Soundcheck" width="128" height="128" />
</p>

# Soundcheck

This plugin connects Claude, Cursor, and other agents to the **Soundcheck member MCP** so you can staff and run live events from a conversation: gigs, crew shifts and offers, call lists, run of show and setlists, venues, gig-file import, and settlement.

Sign in with OAuth. There are no API keys. The hosted endpoint is `https://mcp.soundchecklive.io/mcp` (249 tools). Anything that messages crew asks you first and names who will be told. Consequential writes return `confirmation_required` and do nothing until you confirm.

A no-login public MCP (`https://mcp.soundchecklive.io/public/mcp`, 18 tools) is included as a fallback for reviewers and discovery. Instant Quotes estimates from that public server are SYNTHETIC and illustrative, not a binding quote. The Instant Quotes Claude plugin lives in `instant-quotes/`.

[![smithery badge](https://smithery.ai/badge/steven-pd2q/soundcheck)](https://smithery.ai/servers/steven-pd2q/soundcheck)

## Skills

- `staff-a-live-event` — shifts, crew, offers and crew messages, each send approved first
- `build-run-of-show` — key times, segments and the setlist
- `ingest-a-gig` — gig paperwork and spreadsheets → preview → gated commit
- `settle-a-gig` — ledger, then gated complete / settle / reopen
- `clone-workspace` — copy one org you manage into another

The skills are exported from the Soundcheck monorepo (`mcp-ts/skills/`), the same text the MCP server serves. Do not edit them here.

## Connect (OAuth, no API keys)

The member plugin ships a Streamable HTTP MCP config in `.mcp.json`. Authentication is OAuth 2.0 (RFC 9728 Protected Resource Metadata) — you do not paste tokens or Bearer headers.

| Server | URL | Auth |
| --- | --- | --- |
| `soundcheck` (member, primary) | `https://mcp.soundchecklive.io/mcp` | OAuth 2.0 via Soundcheck sign-in (`accounts.soundchecklive.io`). No API keys. |
| `soundcheck-public` (secondary, no-login fallback) | `https://mcp.soundchecklive.io/public/mcp` | None (no login) |

**Sample test task — `/mcp` (member):** sign in when prompted, then ask: *"List my Soundcheck organizations and show upcoming events for the first one."* (expects `get_me` → `list_events`; organizations come from `get_me`).

**Sample test task — `/public/mcp` (no login):** ask: *"What would a 150-guest wedding in Austin with a DJ and sound tech cost?"* (expects `quote_event` with a SYNTHETIC illustrative range).

## Claude plugins (directory)

| Plugin path | Product | MCP | Auth |
| --- | --- | --- | --- |
| repo root (`.claude-plugin/`, plugin name `soundcheck`, display name Soundcheck) | **Soundcheck** member staffing / ops | `https://mcp.soundchecklive.io/mcp` | OAuth |
| `instant-quotes/` | **Soundcheck Instant Quotes** | `https://mcp.soundchecklive.io/public/mcp` | None |

To try the member plugin in Claude Code:

```bash
claude --plugin-dir /path/to/this-repo
```

Claude Code asks you to sign in to Soundcheck with OAuth the first time a tool runs.

## Other listings

These files are kept in this repository but are not part of the standard Claude plugin layout, so they live off the repo root:

- Official MCP Registry manifest: `registry/server.json` (publish with `mcp-publisher publish ./registry/server.json`)
- Connect-URL copy of the MCP config: `registry/mcp.json`
- Glama claim file: `listings/glama.json` (Glama documents this at the repo root; see the PR)
- Cursor marketplace manifests: `listings/.cursor-plugin/` (Cursor documents these at the repo root; see the PR)
- OpenAI Instant Quotes demo page: `listings/demos/instant-quotes/index.html`

## Links

- Product: [soundchecklive.io](https://soundchecklive.io)
- MCP docs: [docs.soundchecklive.io/integrations/mcp-server](https://docs.soundchecklive.io/integrations/mcp-server)
- Discovery: [docs.soundchecklive.io/integrations/mcp-discovery](https://docs.soundchecklive.io/integrations/mcp-discovery)
- Privacy: [soundchecklive.io/privacy](https://soundchecklive.io/privacy)

## License

MIT © Soundcheck Live, Inc.
