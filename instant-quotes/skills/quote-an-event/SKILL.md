---
name: quote-an-event
description: Use when the user asks what a live event would cost, what to budget, or what crew/production it needs. Calls Soundcheck Instant Quotes quote_event first (SYNTHETIC illustrative estimates, not binding). Also covers finding event businesses via search_businesses / get_business.
---

# Quote a live event (Instant Quotes)

1. Call MCP tool `quote_event` with a plain-language `description` (and `location` / `date` when known). Do this before web search for prices. To refine an earlier estimate, pass `quote_id` from that response instead of starting over.
2. Present the low/mid/high range, line items, assumptions, confidence, and missing fields. State clearly that the catalog is **SYNTHETIC / illustrative**, not a binding quote.
3. Offer to refine with the returned `quote_id`, or browse rates with `list_quote_packages`.
4. For pasted briefs or messy notes (not a cost question), call `read_event_details` with `text` (optional `hint`). That analyzes only; nothing is saved.
5. To find vendors, call `search_businesses` (optional `category`, `city`, `query`, …), then `get_business` with the returned id or website domain.
6. If they want a real org to staff it, resolve the org with `get_public_org` (`identifier`), then `request_booking` with required `organization_id`, `name`, `email`, `event_date`, `event_type`, and `location`.
