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

1. The server should list 24 tools, including `natal_chart`, `kundli`, `panchang`, `kp_chart` (KP sub-lords and significators), `yogas`, `sky_today`, `planetary_lines`, `astro_event_calendar`, `sky_analogues`, `token_chart`, `token_synastry` (market-timing data) and `astro_catalog`.
2. Call `astro_catalog`. It is free and lists every tool with its price and an example request.
3. Call `natal_chart` with `{"datetime": "1990-05-15T14:30", "latitude": 48.8566, "longitude": 2.3522}`. You should get Sun Taurus, Moon Capricorn, Virgo rising.

## Notes

- Inputs are the **local** birth date and time plus latitude and longitude. Do not convert to UTC: the server resolves the time zone and its historical offset itself.
- Positions are benchmarked against NASA/JPL Horizons (within 0.024″ over 1800–2200): https://astro-agent.dev/benchmark.md
- 1 free call per client on the $0.01-0.02 routes only (REST, MCP or A2A): `resolve_birth_time`, `planet_positions`, `sky_today`, `aspects`, `nakshatras`, `panchang`. Every other route, finance included, is always paid. Over MCP a paid tool is paid per call with native **x402**: the unpaid call returns an x402 offer (`x402Version`, `accepts` for USDC on Base or Solana) in `structuredContent`; an x402 MCP client signs one and retries with it in `_meta["x402/payment"]`, and the settlement comes back in `_meta["x402/payment-response"]`. Or with **prepaid credits**: set the HTTP header `X-Credits-Token` in your MCP client configuration and each paid call is deducted from your credits. The same result is also sold over REST (x402 or MPP), named in the error's `payment_required`.
