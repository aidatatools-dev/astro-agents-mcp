Astro Agents brings 24 tools from https://astro-agent.dev/mcp (no account, no API key) and the
`astrology` skill (listed under the plugin's namespace, e.g. `astro-agents:astrology`). Hermes loads plugins only once they are enabled: if the line below says
"installed but not enabled", run `hermes plugins enable astro-agents`. Then start a new session and ask for a birth
chart, a horoscope, a kundli, a KP chart, the yogas in a chart or a panchang with a local date, time and place.
Engine 1.8.3: market-timing data for backtests (`planetary_lines`, `astro_event_calendar`, `sky_analogues`), crypto token
launch charts (`token_chart`, `token_synastry`) and full Western and Vedic astrology.
Positions are benchmarked against NASA/JPL Horizons, within 0.024″: https://astro-agent.dev/benchmark.md

No `mcp__astro_agents__*` tools in the session? Hermes needs its `mcp` extra (part of the standard install; in a
source checkout run `uv sync --extra mcp`). `hermes plugins show astro-agents` should say `Status: enabled`.

1 free call per client on the $0.01-0.02 routes only (REST, MCP or A2A): `resolve_birth_time`, `planet_positions`, `sky_today`, `aspects`, `nakshatras`, `panchang`. Every other route, finance included, is always paid.
Over MCP a paid tool is paid per call with native **x402**: the unpaid call returns an x402 offer (`x402Version`, `accepts` for USDC on Base or Solana) in `structuredContent`; an x402 MCP client signs one and retries with it in `_meta["x402/payment"]`, and the settlement comes back in `_meta["x402/payment-response"]`. Or with **prepaid credits**: set the HTTP header `X-Credits-Token` in your MCP client configuration and each paid call is deducted from your credits.
REST takes x402 or MPP. `astro_catalog` is free and lists every tool with its price.
