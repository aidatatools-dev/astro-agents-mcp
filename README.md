# Astro Agents: astronomical data MCP server (NASA/JPL DE440)

**Research-grade astronomical data & computation API — NASA/JPL DE440 ephemerides for any date, past, present or future, reproducible and SHA-256 verifiable.**

Astro Agents — a research-grade, verifiable astronomical data and computation API for developers, researchers and astronomy, calendar, observation and data applications. From the NASA/JPL DE440 ephemeris: sunrise, sunset and twilight, altitude and azimuth, observation windows, the Moon's phase and distance, the day's sky for a place, planetary positions (geocentric & heliocentric, ecliptic & equatorial), angular separations, longitude time series, eclipses with local circumstances, planetary phenomena, seasons, time scales and coordinate conversion, and a to-the-minute celestial event calendar (solar and lunar eclipses, stations, lunations), 1800-2200. Any date — past, present or future — reproducible: no LLM, historical time zones resolved for you, every result SHA-256 hashed and checkable, accuracy benchmarked against NASA/JPL Horizons. A free key with an email, plans by card, or, for software agents, prepaid credits or pay per call in USDC (x402, MPP). Data only — no predictions, never a signal or forecast.

- **MCP endpoint:** `https://astro-agent.dev/mcp` (remote, Streamable HTTP; nothing to install)
- **Tools:** 20 (18 data tools, plus `astro_catalog` and `verify_result_hash`, free)
- **Docs:** [https://astro-agent.dev/docs](https://astro-agent.dev/docs) · [Quickstart](https://astro-agent.dev/quickstart) · [Playground](https://astro-agent.dev/playground) ·
  [llms.txt](https://astro-agent.dev/llms.txt) · [OpenAPI](https://astro-agent.dev/openapi.json)
- **Accuracy:** benchmarked against NASA/JPL Horizons ([benchmark.md](https://astro-agent.dev/benchmark.md))
- **Tags:** astronomy, ephemeris, nasa-jpl, celestial-events, data

## Connect

Claude Code:

```bash
claude mcp add --transport http astro-agents https://astro-agent.dev/mcp
# with a key (free key on https://astro-agent.dev/pricing):
claude mcp add --transport http astro-agents https://astro-agent.dev/mcp --header "X-Credits-Token: $ASTRO_AGENTS_KEY"
```

Claude (claude.ai and desktop): Settings → Connectors → Add custom connector → `https://astro-agent.dev/mcp`.

Cursor, VS Code, Windsurf and other hosts that take remote servers in JSON (`mcpServers`):

```json
{
  "mcpServers": {
    "astro-agents": {
      "type": "http",
      "url": "https://astro-agent.dev/mcp",
      "headers": {"X-Credits-Token": "<optional key>"}
    }
  }
}
```

Cline: see [llms-install.md](llms-install.md). LangChain / LangGraph (`langchain-mcp-adapters`):

```python
MultiServerMCPClient({"astro": {"transport": "streamable_http", "url": "https://astro-agent.dev/mcp"}})
```

## Tools

### For a place and a day

| Tool | Price | What it returns |
|---|---|---|
| `day_summary` | $0.02 | Today's sky for a place in one call: sunrise, sunset, twilight, golden and blue hour, the Moon's phase and moonrise, the planets visible tonight, the next lunations and the next eclipse. |
| `rise_set_times` | $0.01 | Sunrise, sunset, moonrise, moonset and twilight times for a place and date: rise, transit and set of the Sun, Moon and planets, civil/nautical/astronomical dawn and dusk, day length, golden hour and blue hour, in UTC and local time. |
| `altitude_azimuth` | $0.01 | Where the Sun, Moon and planets are in the sky from a place at a moment: altitude, azimuth, compass point, topocentric right ascension and declination, distance, elongation, illumination, magnitude and the sky state. |
| `observation_windows` | $0.02 | Which planets and the Moon can be observed tonight from a place, and when: windows above a minimum altitude while the sky is dark, with the best time, altitude, azimuth and magnitude. |
| `moon_details` | $0.01 | Moon phase today and in detail: phase name and angle, illuminated fraction, age, distance, apparent size, the next new, quarter and full Moons, and the next perigee and apogee; with a place, moonrise and moonset. |
| `solunar_periods` | $0.01 | Solunar periods for a place and day: major periods around the Moon's upper and lower transits, minor periods around moonrise and moonset, in local time. |

### Positions and angles

| Tool | Price | What it returns |
|---|---|---|
| `body_positions` | $0.01 | Planetary positions and ephemeris API (NASA/JPL DE440): the ecliptic longitude, latitude, speed, retrograde flag and distance of the Sun, Moon, planets and lunar nodes at any moment 1800-2200, geocentric or heliocentric, ecliptic or equatorial (right ascension, declination). |
| `sky_now` | $0.01 | The sky now (no location needed): planet positions, retrograde planets and the Moon phase, plus the exact UTC minute of the next stations, lunations and separations. |
| `angular_separations` | $0.02 | Angular separations between planets at a moment, or between longitudes you supply: the pairs near 0, 60, 90, 120 or 180 degrees (or any angle you choose) with the deviation, approaching or receding; optionally equal and opposite declinations. |
| `resolve_local_time` | $0.01 | Local time to UTC conversion: convert a local date, time and place into exact Universal Time, with the historical time zone and UTC offset, Delta T, Local Mean Time and true solar time. |

### Calendars and long ranges

| Tool | Price | What it returns |
|---|---|---|
| `celestial_events` | $0.25 | Celestial event calendar: the exact UTC minute and unix timestamp of every solar and lunar eclipse, planetary station (retrograde / direct), lunation and separation between two dates, 1800-2200. |
| `planetary_phenomena` | $0.10 | Planetary phenomena between two dates: oppositions, conjunctions with the Sun, greatest elongations of Mercury and Venus, conjunctions between planets, perihelion and aphelion. |
| `local_eclipse` | $0.10 | Solar and lunar eclipses seen from a place: local type, contact times (C1-C4, P1-P4, U1-U4), magnitude, obscuration, duration of totality and the Sun's or Moon's altitude. |
| `seasons` | $0.01 | Equinoxes and solstices of a year, Earth's perihelion and aphelion, and optionally the Sun day by day (sunrise, sunset, day length, declination, equation of time) for a place. |
| `longitude_series` | $0.20 | Planetary longitude time series: planet degrees seen from the Earth and from the Sun (geocentric and heliocentric) at a moment or every hour to every week, as pandas-ready columns, with speeds, retrograde flags and declinations. |
| `similar_skies` | $0.25 | Sky similarity search: find the past or future dates whose planetary positions best match a reference sky, ranked by a stated angular distance in degrees. |

### Time and coordinates

| Tool | Price | What it returns |
|---|---|---|
| `time_scales` | $0.01 | Julian day, modified Julian day, sidereal time (Greenwich and local, mean and apparent), Delta T, TAI-UTC and UT1-UTC for a moment. |
| `convert_coordinates` | $0.01 | Convert sky coordinates between equatorial (J2000, of date), ecliptic, galactic and horizontal (alt-az) frames. |

`astro_catalog` (free) lists every tool with its price and an example; `verify_result_hash` (free) recomputes a
result's SHA-256.

## Example

`day_summary` with `{"date": "2026-08-10", "latitude": 48.8566, "longitude": 2.3522}` returns the Sun (sunrise, sunset, twilight, golden hour), the Moon (phase,
illumination, moonrise), the planets observable that night and the next eclipse, in local time with offsets. Full
real outputs: [examples/](examples/).

## Pricing and keys

- **Free:** each client gets 1 free call on the $0.01-0.02 tools; a free key (Free plan on [https://astro-agent.dev/pricing](https://astro-agent.dev/pricing))
  gives 10,000 calls a month on the core tools and 100 on the premium data tools.
- **With a key:** set the HTTP header `X-Credits-Token` in your MCP client configuration (a plan key or a prepaid-credits token).
- **Pay per call:** an unpaid paid tool returns a standard x402 offer (USDC on Base or Solana) in `structuredContent`;
  an x402 MCP client pays and retries with it in `_meta["x402/payment"]`.

## Reproducible

No language model computes anything: positions come from NASA/JPL DE440, and every result carries
`meta.input_sha256` and `meta.result_sha256`. The same request returns the same bytes; `verify_result_hash` (or
`POST https://astro-agent.dev/v1/verify`) recomputes the hash. Every convention (frame, refraction, horizon, twilight depth) is stated
in the result. Data only: no predictions, never a signal or forecast.

## Server details

| | |
|---|---|
| Name in the MCP Registry | `io.github.aidatatools-dev/astro-agents` |
| Version | 1.10.0 |
| Transport | Streamable HTTP, stateless |
| Authentication | none required; optional `X-Credits-Token` header |
| Ephemeris | NASA/JPL DE440, 1800-2200 |
| Licence | MIT (this repository); the hosted service has its own [terms](https://astro-agent.dev/terms) |

Support: https://astro-agent.dev/support
