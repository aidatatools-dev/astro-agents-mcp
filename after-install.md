Astro Agents brings 23 astronomy tools from https://astro-agent.dev/mcp (no account needed). If the line below says
"installed but not enabled", run `hermes plugins enable astro-agents`, then start a new session and ask for
today's sky, sunrise and sunset, the Moon's phase, planet positions or the next eclipse for a place.
Positions are benchmarked against NASA/JPL Horizons: https://astro-agent.dev/benchmark.md

No `mcp__astro_agents__*` tools in the session? Hermes needs its `mcp` extra (part of the standard install; in a
source checkout run `uv sync --extra mcp`). `hermes plugins show astro-agents` should say `Status: enabled`.

1 free call per client on the $0.01-0.02 tools; a free key on https://astro-agent.dev/pricing (header `X-Credits-Token`) gives
10,000 calls a month. `astro_catalog` is free and lists every tool with its price.
