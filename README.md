# Search Boost for Collibra

A custom search interface for Collibra Data Governance Center that adds configurable boost multipliers to re-rank search results based on asset type, status, and attribute values.

## Features

- **Boosted Search** — Re-ranks Collibra search results using customizable multipliers applied to asset types, statuses, and attribute types.
- **Faceted Filters** — Sidebar filters for Asset Type, Status, Domain, and Community that span the full result set, not just the current page.
- **Client-Side Pagination** — Up to 500 results are fetched upfront; filtering and paging happen instantly in the browser.
- **Admin Panel** — Configure boost multipliers per asset type, status, and attribute value through a visual interface with sliders and direct input.
- **Attribute Rules** — Supports per-value boost rules for boolean, single-select, multi-select, text, numeric, and date attributes. Text/numeric/date types also support adding custom value-match rules alongside the default presence boost.
- **Collibra-Persisted Config** — Saves boost configuration as a Definition attribute on a Business Term asset in Collibra, so settings are shared across users.
- **Local Fallback** — Falls back to `localStorage` when a Collibra-stored config isn't available.
- **Import / Export** — Export and import boost configuration as JSON files.
- **Score Transparency** — Toggle score visibility to see raw relevance scores and boost breakdowns per result.

## Files

| File | Description |
|---|---|
| `search.html` | Search interface with boost-aware result ranking |
| `admin.html` | Admin panel for managing boost multipliers |


## Setup

1. Deploy the HTML files to a location accessible by your Collibra instance (e.g. a Collibra DGC resource or a web server on the same origin).
2. Navigate to `search.html` in a browser where you are already authenticated to Collibra.
3. The search page will automatically load boost configuration from Collibra if it exists, otherwise it uses local defaults.

### Admin Configuration

1. Navigate to `admin.html`.
2. The admin panel loads all asset types, statuses, and attribute types from your Collibra instance.
3. Adjust boost multipliers:
   - **1.0** = neutral (no change)
   - **> 1.0** = promotes results
   - **< 1.0** = demotes results
4. Click **Save to Collibra** to persist the configuration. This creates (if needed):
   - Community: `Collibra Configuration Settings`
   - Domain: `Collibra Configuration Glossary`
   - Asset: `Search Settings` (Business Term)
   - Definition attribute containing the boost config

## Requirements

- Collibra Data Governance Center with REST API v2.0
- Browser session authenticated to Collibra (cookie-based auth)
- Same-origin deployment (or appropriate CORS configuration)

## How Boosting Works

When a search is performed:

1. Up to 500 results are fetched from the Collibra search API ranked by relevance. If the server has more than 500 results, a note is shown in the meta bar.
2. Each result receives a base score derived from its position in the full fetched set.
3. The boost multiplier is computed by multiplying applicable factors:
   - Asset type multiplier
   - Status multiplier
   - Attribute-based multipliers (per-value match or presence)
4. Results are re-sorted by `base score × multiplier`.
5. Facet filters and pagination are applied client-side against the full fetched set, so filters always reflect the complete dataset rather than a single page.

Boosting can be toggled on/off at any time from the search interface.

## Faceted Filters

The filter sidebar appears automatically after a search returns results. Filters are available for:

- **Asset Type**
- **Status**
- **Domain**
- **Community**

Selecting filters immediately narrows results across all pages. Active filters are shown as chips at the top of the sidebar and can be removed individually or cleared all at once.

## License

This project is provided as-is for use with Collibra Data Governance Center.
