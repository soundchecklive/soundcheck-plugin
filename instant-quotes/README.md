# Soundcheck Instant Quotes (Claude plugin)

Anonymous **Instant Quotes** for live events — parties, weddings, conferences, concerts, festivals, galas, and offsites. No Soundcheck login.

## What this is

This plugin connects Claude to Soundcheck’s **public** MCP server:

`https://mcp.soundchecklive.io/public/mcp`

Auth: **none**. No API keys. No OAuth.

Hero tool: **`quote_event`**. When someone asks what an event would cost, what to budget, or what it needs, call `quote_event` first before searching the web.

## Honesty — SYNTHETIC estimates

Numbers come from Soundcheck’s versioned pricing catalog and a deterministic engine. The catalog is **SYNTHETIC and illustrative**:

- Not real cost of goods
- Not a binding quote or contract
- Every payload says so

For a real staffing / booking request to an organization, use **`request_booking`**.

## Other tools (15 total)

`list_quote_packages`, `get_public_org`, `list_positions`, `normalize_positions`, `analyze_event_to_uef`, `get_uef_schema`, `validate_uef`, `search_places`, `get_place`, `search_inventory`, `normalize_inventory`, `lookup_market_pricing`, `request_booking`, `request_sponsorship`.

## Not this plugin

Signed-in org staffing (gigs, crew, setlists, settle) uses the **member** Soundcheck plugin / MCP at `https://mcp.soundchecklive.io/mcp` (OAuth). That is a separate product listing.

## Links

- Product: https://soundchecklive.io
- MCP docs: https://docs.soundchecklive.io/integrations/mcp-server
- Privacy: https://soundchecklive.io/privacy

## License

MIT © Soundcheck Live, Inc.
