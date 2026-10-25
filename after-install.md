Astro Agents brings 16 astrology tools from https://astro-agent.dev/mcp (no account, no API key) and the
`astrology` skill (listed under the plugin's namespace, e.g. `astro-agents:astrology`). Hermes loads plugins only once they are enabled: if the line below says
"installed but not enabled", run `hermes plugins enable astro-agents`. Then start a new session and ask for a birth
chart, a horoscope, a kundli or a panchang with a local date, time and place.

No `mcp__astro_agents__*` tools in the session? Hermes needs its `mcp` extra (part of the standard install; in a
source checkout run `uv sync --extra mcp`). `hermes plugins show astro-agents` should say `Status: enabled`.

Each client gets 3 free tool calls. After that a tool explains how to pay per call over REST (x402 or MPP);
nothing is ever charged over MCP. `astro_catalog` is free and lists every tool with its price.
