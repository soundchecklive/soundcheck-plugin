---
name: quote-an-event
description: Researching a live event through the Soundcheck public directory - finding event vendors and talent by category and city, looking up well-known venues and hotels, normalizing crew roles and gear, and checking market price ranges or an illustrative (SYNTHETIC, non-binding) cost estimate. Covers search_businesses, get_business, search_places, get_place, lookup_market_pricing and quote_event.
---

# Research a live event (Soundcheck public directory)

The skill keeps its original `quote-an-event` name for compatibility; it covers directory research first and estimates second.

1. **Vendors and talent.** `search_businesses` takes an optional `category` (for example `sound`, `dj`, `catering`), `city` / `region` / `country` and free-text `query`. `get_business` returns one listing by the returned id or a website domain: website, services, published contact details and source URLs. Coverage varies by city and category; an empty result means no listings yet for that search, and it is reported as such rather than filled in.
2. **Venues and hotels.** `search_places` and `get_place` cover a seeded catalog of well-known venues and hotels. Unknown names come back empty with suggestions; addresses are never invented.
3. **Organizations on Soundcheck.** `get_public_org` returns a public profile by slug or id.
4. **Standard event data.** `normalize_positions` maps crew labels (FOH, A2, LD) to catalog positions; `normalize_inventory` maps gear lines to inventory ids. For pasted briefs or messy notes, `read_event_details` (`text`, optional `hint`) drafts a Universal Event Format file and lists missing fields; `check_import_file` checks a draft. Nothing is saved.
5. **Pricing.** `lookup_market_pricing` returns evidence-backed ranges with citations, or `coverage: "none"` when evidence is missing. For a ballpark, `quote_event` takes a plain-language `description` (plus `location` / `date` when known) and returns an illustrative low/mid/high range, line items, assumptions, confidence and missing fields. Its catalog is **SYNTHETIC / illustrative**, not a binding quote, and the estimate is presented that way. `quote_id` refines an earlier estimate; `list_quote_packages` shows the rates behind it.
6. **Requests.** Only when the person asks to contact an organization: resolve it with `get_public_org`, then `request_booking` needs `organization_id`, `name`, `email`, `event_date`, `event_type` and `location`. The request is sent only after that person confirms by email.
