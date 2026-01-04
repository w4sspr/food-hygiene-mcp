# Food Hygiene MCP Server

Query UK food hygiene ratings from Claude.

[![Python 3.14+](https://img.shields.io/badge/python-3.14+-blue.svg)](https://www.python.org/downloads/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![MCP](https://img.shields.io/badge/MCP-Server-green.svg)](https://modelcontextprotocol.io)

<!--
## Demo

![Demo GIF](demo.gif)

TODO: Record a ~10 second GIF showing Claude Desktop answering "What's the hygiene rating for Dishoom in London?"
-->

## Quick Start

```bash
# Clone and install
git clone https://github.com/w4sspr/food-hygiene-mcp.git
cd food-hygiene-mcp
uv sync

# Add to Claude Desktop config (~/.config/claude/claude_desktop_config.json)
```

```json
{
  "mcpServers": {
    "food-hygiene": {
      "command": "uv",
      "args": ["run", "--directory", "/path/to/food-hygiene-mcp", "food-hygiene-mcp"]
    }
  }
}
```

Restart Claude Desktop, then try:

> "What's the hygiene rating for Dishoom in London?"

## Example Prompts

- "What's the hygiene rating for Dishoom in London?"
- "Find restaurants in Manchester with at least a 4-star rating"
- "Show me takeaways in SW1A 1AA"
- "Get full inspection details for establishment 1662145"
- "Find cafes within 2 miles of 51.5074, -0.1278"

## Tools

### `search_establishments`

Search for food establishments by name, location, or rating.

| Parameter | Type | Description |
|-----------|------|-------------|
| `name` | string | Business name (partial match) |
| `address` | string | Street or area |
| `postcode` | string | UK postcode |
| `latitude` | float | Latitude for geo-search |
| `longitude` | float | Longitude for geo-search |
| `radius_miles` | float | Max distance in miles (default: 1) |
| `business_type` | string | "restaurant", "takeaway", "pub", "cafe", "hotel", etc. |
| `min_rating` | int | Minimum hygiene rating (0-5) |
| `max_rating` | int | Maximum hygiene rating (0-5) |

### `get_establishment_details`

Get full details for a specific establishment by FHRS ID.

Returns: rating breakdown (hygiene, structural, confidence in management), inspection date, local authority info.

## Limitations

- **England, Wales, and Northern Ireland only** — Scotland uses a different system
- **No aggregate statistics** — can't calculate "% of 5-star restaurants" without fetching all data
- **Limited business types** — ~15 common types mapped; niche categories may not work
- **Geo-search requires coordinates** — city names alone won't trigger distance filtering

## Roadmap

- [ ] `get_local_authority_stats` — aggregate ratings by area (requires pagination through all establishments)
- [ ] Scotland support via FHIS API
- [ ] Cache business type mappings

## Why I Built This

Built to demonstrate MCP integration with UK public sector data for my FDE application.

## See Also

- [uk-charities-mcp](https://github.com/w4sspr/uk-charities-mcp) — Query the Charity Commission register

## Data Source

[FSA Food Hygiene Rating Scheme API](https://api.ratings.food.gov.uk/Help) — free, public, no authentication required.

## License

MIT
