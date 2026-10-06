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

1. The server should list 16 tools, including `natal_chart`, `kundli`, `panchang` and `astro_catalog`.
2. Call `astro_catalog`. It is free and lists every tool with its price and an example request.
3. Call `natal_chart` with `{"datetime": "1990-05-15T14:30", "latitude": 48.8566, "longitude": 2.3522}`. You should get Sun Taurus, Moon Capricorn, Virgo rising.

## Notes

- Inputs are the **local** birth date and time plus latitude and longitude. Do not convert to UTC: the server resolves the time zone and its historical offset itself.
- Each client gets 3 free tool calls. After that a tool returns an error with `payment_required`, which gives the paid REST endpoint, its price and how to pay (x402 or MPP). Nothing is ever charged over MCP.
