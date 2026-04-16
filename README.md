# Search Boost for Collibra

A custom search interface for Collibra Data Governance Center that adds configurable boost multipliers to re-rank search results based on asset type, status, and attribute values.

## Features

- **Boosted Search** — Re-ranks Collibra search results using customizable multipliers applied to asset types, statuses, and attribute types.
- **Admin Panel** — Configure boost multipliers per asset type, status, and attribute value through a visual interface with sliders and direct input.
- **Attribute Rules** — Supports per-value boost rules for boolean, single-select, multi-select, and text/numeric attributes (boost on presence).
- **Collibra-Persisted Config** — Saves boost configuration as a Definition attribute on a Business Term asset in Collibra, so settings are shared across users.
- **Local Fallback** — Falls back to `localStorage` when a Collibra-stored config isn't available.
- **Import / Export** — Export and import boost configuration as JSON files.
- **Score Transparency** — Toggle score visibility to see raw relevance scores and boost breakdowns per result.

## Files

| File | Description |
|---|---|
| `search.html` | Search interface with boost-aware result ranking |
| `admin.html` | Admin panel for managing boost multipliers |
| `collibra-rest-schema.json` | Collibra REST API v2.0 OpenAPI schema (reference) |

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

1. Results are fetched from the Collibra search API ranked by relevance.
2. Each result receives a base score derived from its position.
3. The boost multiplier is computed by multiplying applicable factors:
   - Asset type multiplier
   - Status multiplier
   - Attribute-based multipliers (per-value match or presence)
4. Results are re-sorted by `base score × multiplier`.

Boosting can be toggled on/off at any time from the search interface.

## License

This project is provided as-is for use with Collibra Data Governance Center.
