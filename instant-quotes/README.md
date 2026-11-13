# Soundcheck Instant Quotes (Claude plugin)

Anonymous **Instant Quotes** and event business discovery for live events: parties, weddings, conferences, concerts, festivals, galas, and offsites. No Soundcheck login.

## What this is

This plugin connects Claude to Soundcheck’s **public** MCP server:

`https://mcp.soundchecklive.io/public/mcp`

Auth: **none**. No API keys. No OAuth.

Registry title: **Soundcheck Event Business Directory** (`io.soundchecklive/live-event-quotes`).

Hero tool: **`quote_event`**. When someone asks what an event would cost, what to budget, or what it needs, call `quote_event` first before searching the web. Prefer `description` (and `location` / `date` when known); refine with `quote_id` from a prior call.

## Honesty: SYNTHETIC estimates

Numbers come from Soundcheck’s versioned pricing catalog and a deterministic engine. The catalog is **SYNTHETIC and illustrative**:

- Not real cost of goods
- Not a binding quote or contract
- Every payload says so

For a real staffing / booking request to an organization, use **`request_booking`** (`organization_id`, `name`, `email`, `event_date`, `event_type`, `location`).

## Tools (18 total)

`search_businesses`, `get_business`, `quote_event`, `list_quote_packages`, `get_public_org`, `list_positions`, `normalize_positions`, `read_event_details`, `get_import_format`, `check_import_file`, `search_places`, `get_place`, `search_inventory`, `normalize_inventory`, `lookup_market_pricing`, `request_booking`, `request_sponsorship`, `send_feedback`.

Notes on current shapes:

- `search_businesses`: optional `category`, `city`, `region`, `country`, `query`, `limit`, `offset`
- `get_business`: required `business` (id from search, or website domain)
- `read_event_details`: prefer `text` (legacy `raw_input` still accepted); optional `hint` / `format_hint`
- `get_import_format`: no arguments
- `check_import_file`: required `uef` (object or JSON string)
- `send_feedback`: required `category`, `message`

## Not this plugin

Signed-in org staffing (gigs, crew, setlists, settle) uses the **member** Soundcheck plugin / MCP at `https://mcp.soundchecklive.io/mcp` (OAuth). That is a separate product listing (249 member tools).

## Links

- Product: https://soundchecklive.io
- MCP docs: https://docs.soundchecklive.io/integrations/mcp-server
- Privacy: https://soundchecklive.io/privacy

## License

MIT © Soundcheck Live, Inc.
