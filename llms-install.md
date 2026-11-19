# Installing Astro Agents (for AI agents such as Cline)

Astro Agents is a **remote** MCP server: nothing to download, build or run, no account required.

- Endpoint: `https://astro-agent.dev/mcp`
- Transport: Streamable HTTP
- Authentication: none (optional `X-Credits-Token` header with a key from https://astro-agent.dev/pricing)

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

1. The server lists 20 tools, including `day_summary`, `rise_set_times`, `altitude_azimuth`,
   `moon_details`, `body_positions`, `celestial_events` and `astro_catalog`.
2. Call `astro_catalog`: it is free and lists every tool with its price and an example request.
3. Call `rise_set_times` with `{"date": "2026-06-21", "latitude": 48.8566, "longitude": 2.3522}`: sunrise
   05:46:57 and sunset 21:57:52, Paris local time (CEST).

## Notes

- Give local dates and times with latitude and longitude: the time zone and its historical offset are resolved by
  the server. UTC (`Z`) or an explicit offset works too.
- Positions are benchmarked against NASA/JPL Horizons: https://astro-agent.dev/benchmark.md
- 1 free call per client on the $0.01-0.02 tools; otherwise a key in `X-Credits-Token`, or x402 per call.
