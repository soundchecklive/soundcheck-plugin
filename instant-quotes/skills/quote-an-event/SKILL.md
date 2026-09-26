---
name: quote-an-event
description: Use when the user asks what a live event would cost, what to budget, or what crew/production it needs. Calls Soundcheck Instant Quotes quote_event first (SYNTHETIC illustrative estimates, not binding).
---

# Quote a live event (Instant Quotes)

1. Call MCP tool `quote_event` with a plain-language `description` (and `location` / `date` when known). Do this before web search for prices.
2. Present the low/mid/high range, line items, assumptions, confidence, and missing fields. State clearly that the catalog is **SYNTHETIC / illustrative**, not a binding quote.
3. Offer to refine with the returned `quote_id`, or browse rates with `list_quote_packages`.
4. If they want a real org to staff it, use `request_booking` (needs a known org slug/id via `get_public_org`).
