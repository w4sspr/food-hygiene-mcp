# Food Hygiene MCP Server

An MCP server that lets Claude answer questions about food hygiene ratings in England, Wales, and Northern Ireland using the [FSA Food Hygiene Rating Scheme API](https://ratings.food.gov.uk/).

## What it does

- Search for food establishments by name, location, postcode, or rating
- Get detailed hygiene inspection information for any establishment
- Filter by business type (restaurants, takeaways, pubs, cafes, etc.)
- Find highly-rated places near specific coordinates

## Installation

Requires Python 3.14+ and [uv](https://docs.astral.sh/uv/).

```bash
git clone https://github.com/w4sspr/food-hygiene-mcp.git
cd food-hygiene-mcp
uv sync
```

## Claude Desktop Configuration

Add to your `claude_desktop_config.json`:

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

Replace `/path/to/food-hygiene-mcp` with the actual path.

## Example Prompts

- "What's the hygiene rating for Dishoom in London?"
- "Find restaurants in Manchester with at least a 4-star rating"
- "Show me takeaways in SW1A 1AA with poor ratings"
- "Get full inspection details for establishment 1662145"
- "Find cafes within 2 miles of 51.5074, -0.1278"
- "What pubs near Birmingham have a 5-star hygiene rating?"

## Tools

### `search_establishments`

Search for food establishments by various criteria.

| Parameter | Type | Description |
|-----------|------|-------------|
| `name` | string | Business name (partial match) |
| `address` | string | Street or area |
| `postcode` | string | UK postcode |
| `latitude` | float | Latitude for geo-search |
| `longitude` | float | Longitude for geo-search |
| `radius_miles` | float | Max distance in miles (default: 1) |
| `business_type` | string | "restaurant", "takeaway", "pub", "cafe", "hotel", "supermarket", etc. |
| `min_rating` | int | Minimum hygiene rating (0-5) |
| `max_rating` | int | Maximum hygiene rating (0-5) |
| `page` | int | Page number (default: 1) |
| `page_size` | int | Results per page (default: 10, max: 100) |

### `get_establishment_details`

Get full details for a specific establishment.

| Parameter | Type | Description |
|-----------|------|-------------|
| `fhrs_id` | int | Establishment ID (from search results) |

Returns rating breakdown (hygiene, structural, confidence in management scores), inspection date, local authority info, and more.

## Limitations

- **England, Wales, and Northern Ireland only** - Scotland uses a different system (Food Hygiene Information Scheme) with Pass/Improvement Required ratings
- **No real-time data** - Ratings update when the FSA API updates, usually within days of an inspection
- **No aggregate statistics** - Can't calculate "% of 5-star restaurants in an area" without fetching all establishments
- **Limited business type matching** - Only ~15 common types are mapped; niche categories may not work
- **No historical data** - Only shows the current rating, not inspection history
- **Geo-search requires coordinates** - "Near me" queries need lat/long; city names alone won't trigger distance filtering

## Data Source

This server uses the [FSA Food Hygiene Rating Scheme API](https://api.ratings.food.gov.uk/Help), which is free, public, and requires no authentication.

## License

MIT
