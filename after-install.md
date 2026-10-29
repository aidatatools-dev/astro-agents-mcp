Astro Agents brings 21 astrology tools from https://astro-agent.dev/mcp (no account, no API key) and the
`astrology` skill (listed under the plugin's namespace, e.g. `astro-agents:astrology`). Hermes loads plugins only once they are enabled: if the line below says
"installed but not enabled", run `hermes plugins enable astro-agents`. Then start a new session and ask for a birth
chart, a horoscope, a kundli, a KP chart, the yogas in a chart or a panchang with a local date, time and place.
New in 1.4.0: financial-astrology data for backtests (`planetary_lines`, `astro_event_calendar`) and `sky_today`.
Positions are benchmarked against NASA/JPL Horizons, within 0.024″: https://astro-agent.dev/benchmark.md

No `mcp__astro_agents__*` tools in the session? Hermes needs its `mcp` extra (part of the standard install; in a
source checkout run `uv sync --extra mcp`). `hermes plugins show astro-agents` should say `Status: enabled`.

Each client gets 1 free call, on the $0.01–0.02 tools only (`resolve_birth_time`, `planet_positions`, `sky_today`, `aspects`, `nakshatras` or `panchang`); every other tool,
finance included, is always paid. A paid tool explains how to pay per call over REST (x402 or MPP);
nothing is ever charged over MCP. `astro_catalog` is free and lists every tool with its price.
