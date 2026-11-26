<p align="center">
  <img src="assets/logo.png" alt="Soundcheck Live" width="128" height="128" />
</p>

# Soundcheck Live

**[Soundcheck Live](https://soundchecklive.io)** is staffing software for live events: book gigs, send shift offers to freelance crew, share the run of show on crew phones, and track and pay crew per gig. The app and its connectors shorten the name to "Soundcheck".

**Links:** [Soundcheck Live](https://soundchecklive.io) · [Pricing](https://soundchecklive.io/pricing) (14-day free trial, no card) · [MCP docs](https://docs.soundchecklive.io/integrations/mcp-server) · [All AI assistants](https://soundchecklive.io/mcp) · [Facts for AI assistants](https://soundchecklive.io/ai-info) · [Privacy](https://soundchecklive.io/privacy) · Support: info@soundchecklive.io

This plugin connects Claude to the **Soundcheck Live member MCP** so you can staff and run live events from a conversation: gigs, crew shifts and offers, call lists, run of show and setlists, venues, gig-file import, and settlement.

Sign in with OAuth. There are no API keys. The hosted endpoint is `https://mcp.soundchecklive.io/mcp` (249 tools). Anything that messages crew asks you first and names who will be told. Consequential writes return `confirmation_required` and do nothing until you confirm.

A no-login public MCP (`https://mcp.soundchecklive.io/public/mcp`, 18 tools) is included as a fallback for reviewers and discovery. Instant Quotes estimates from that public server are SYNTHETIC and illustrative, not a binding quote.

## Skills

- `staff-a-live-event` — shifts, crew, offers and crew messages, each send approved first
- `build-run-of-show` — key times, segments and the setlist
- `ingest-a-gig` — gig paperwork and spreadsheets → preview → gated commit
- `settle-a-gig` — ledger, then gated complete / settle / reopen
- `clone-workspace` — copy one org you manage into another

The skills are exported from the Soundcheck monorepo (`mcp-ts/skills/`), the same text the MCP server serves. Do not edit them here.

## Links

- Product: [soundchecklive.io](https://soundchecklive.io)
- MCP docs: [docs.soundchecklive.io/integrations/mcp-server](https://docs.soundchecklive.io/integrations/mcp-server)
- Privacy: [soundchecklive.io/privacy](https://soundchecklive.io/privacy)

## License

MIT © Soundcheck Live, Inc.
