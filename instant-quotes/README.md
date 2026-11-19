# Soundcheck Event Directory (public MCP, no login)

Soundcheck is live event digital infrastructure: the primitives agents need to manage any type of event. This plugin connects Claude to Soundcheck's **public** MCP server, an anonymous live event directory and the data standards behind it. No Soundcheck login.

## What this is

`https://mcp.soundchecklive.io/public/mcp`

Auth: **none**. No API keys. No OAuth.

Name: **Soundcheck Event Directory** (registry id `io.soundchecklive/live-event-quotes`). The member server is **Soundcheck: Live Event Ops**. The plugin keeps its original `soundcheck-instant-quotes` name and `quote-an-event` skill name so existing installs and listings keep working.

## What it covers

**Directory**

- `search_businesses`: event businesses (AV and production companies, sound, lighting, staging, backline, video, DJs, live bands, entertainment agencies, planners, caterers, photographers, videographers, rentals, decor, staffing, security and transportation) by `category`, `city` / `region` / `country` and free-text `query`. Listings are researched from public websites and cached, so coverage varies by city and category.
- `get_business`: one listing by the id `search_businesses` returned or by website domain: website, services, published contact details, source URLs, and whether the business is on Soundcheck.
- `search_places` / `get_place`: a seeded catalog of well-known venues and hotels (not a complete world database; unknown names return empty results, never invented addresses).
- `get_public_org`: the public profile of an organization on Soundcheck.

**Standard event data**

- `list_positions` / `normalize_positions`: one catalog of crew roles; free-text labels such as FOH, A2 or LD map onto it.
- `search_inventory` / `normalize_inventory`: one taxonomy of live-event gear; free-text gear lines map onto stable ids.
- `read_event_details`, `get_import_format`, `check_import_file`: turn messy event material into a Universal Event Format (UEF) draft, read the UEF schema, and check a draft. Nothing is saved.

**Pricing**

- `lookup_market_pricing`: evidence-backed price ranges with citations; `coverage: "none"` and no number when evidence is missing.
- `quote_event` / `list_quote_packages`: illustrative estimates from a versioned **SYNTHETIC** catalog and a deterministic engine. Not real cost of goods, not a binding quote or contract; every payload says so.

**Requests and feedback**

- `request_booking` / `request_sponsorship`: send a request to an organization on Soundcheck. Nothing is sent until the requester opens the confirmation email and clicks Confirm.
- `send_feedback`: bug reports, suggestions, questions and praise for the Soundcheck team.

## Tools (18 total)

`search_businesses`, `get_business`, `quote_event`, `list_quote_packages`, `get_public_org`, `list_positions`, `normalize_positions`, `read_event_details`, `get_import_format`, `check_import_file`, `search_places`, `get_place`, `search_inventory`, `normalize_inventory`, `lookup_market_pricing`, `request_booking`, `request_sponsorship`, `send_feedback`.

Notes on current shapes:

- `search_businesses`: optional `category`, `city`, `region`, `country`, `query`, `limit`, `offset`
- `get_business`: required `business` (id from search, or website domain)
- `read_event_details`: `text` (legacy `raw_input` still accepted); optional `hint` / `format_hint`
- `get_import_format`: no arguments
- `check_import_file`: required `uef` (object or JSON string)
- `quote_event`: plain-language `description`, optional `location` / `date`; `quote_id` refines an earlier estimate
- `send_feedback`: required `category`, `message`

## Not this plugin

Signed-in org staffing (gigs, crew, setlists, settle) uses the **member** Soundcheck plugin / MCP at `https://mcp.soundchecklive.io/mcp` (OAuth). That is a separate product listing (249 member tools).

## Links

- Product: https://soundchecklive.io
- MCP docs: https://docs.soundchecklive.io/integrations/mcp-server
- Privacy: https://soundchecklive.io/privacy

## License

MIT © Soundcheck Live, Inc.
