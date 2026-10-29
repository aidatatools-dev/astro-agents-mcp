# Installing Astro Agents (for AI agents such as Cline)

Astro Agents is a **remote** MCP server: there is nothing to download, build or run locally, and no API key.

- Endpoint: `https://astro-agent.dev/mcp`
- Transport: Streamable HTTP
- Authentication: none

## Cline

Add this entry to `cline_mcp_settings.json` (Cline → MCP Servers → Configure), then save:

```json
{
  "mcpServers": {
    "astro-agents": {
      "type": "streamableHttp",
      "url": "https://astro-agent.dev/mcp",
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

## Check that it works

1. The server should list 21 tools, including `natal_chart`, `kundli`, `panchang`, `kp_chart` (KP sub-lords and significators), `yogas`, `sky_today`, `planetary_lines`, `astro_event_calendar` (financial-astrology data) and `astro_catalog`.
2. Call `astro_catalog`. It is free and lists every tool with its price and an example request.
3. Call `natal_chart` with `{"datetime": "1990-05-15T14:30", "latitude": 48.8566, "longitude": 2.3522}`. You should get Sun Taurus, Moon Capricorn, Virgo rising.

## Notes

- Inputs are the **local** birth date and time plus latitude and longitude. Do not convert to UTC: the server resolves the time zone and its historical offset itself.
- Positions are benchmarked against NASA/JPL Horizons (within 0.024″ over 1800–2200): https://astro-agent.dev/benchmark.md
- Each client gets 1 free call, on the $0.01–0.02 tools only (`resolve_birth_time`, `planet_positions`, `sky_today`, `aspects`, `nakshatras` or `panchang`). Every other tool, finance included, is always paid: it returns an error with `payment_required`, which gives the paid REST endpoint, its price and how to pay (x402 or MPP). Nothing is ever charged over MCP.
