# Astro Agents: astronomical data MCP server (NASA/JPL DE440)

**Research-grade astronomical data & computation API — NASA/JPL DE440 ephemerides for any date, past, present or future, reproducible and SHA-256 verifiable.**

Astro Agents — a research-grade, verifiable astronomical data and computation API for developers, researchers and astronomy, calendar, observation and data applications. Ephemerides from the real NASA/JPL DE440: planetary positions (geocentric & heliocentric, ecliptic & equatorial: right ascension and declination), speeds and retrograde motion, Moon phase and illumination, angular separations between bodies (aspects, declination parallels), longitude time series, and a to-the-minute celestial event calendar (solar and lunar eclipses, stations, ingresses, lunations, out-of-bounds), 1800-2200. Any date — past, present or future — reproducible: no LLM, historical time zones resolved for you, every result SHA-256 hashed and checkable, accuracy benchmarked against NASA/JPL Horizons. Pay per call over REST or natively in MCP with x402 (USDC on Base or Solana) or MPP: zero setup, no account; or with prepaid credits or a monthly plan. Data only — no predictions, never a signal or forecast.

- **MCP endpoint:** `https://astro-agent.dev/mcp` (remote, Streamable HTTP; nothing to install)
- **Tools:** 23 (21 data tools, plus `astro_catalog` and `verify_result_hash`, free)
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
| `planet_positions` | $0.01 | Planetary positions and ephemeris API (NASA/JPL DE440): the longitude, sign, latitude, speed and retrograde flag of the Sun, Moon, planets, lunar nodes and Lilith at any moment, geocentric or heliocentric, ecliptic or equatorial (declination, out-of-bounds). |
| `sky_today` | $0.01 | The sky now (no location needed): planet positions, retrogrades and the Moon phase now, plus the exact times of the next ingresses, Mercury retrograde and other stations, new and full moons and planetary aspects over the coming days. |
| `aspects` | $0.02 | Angular separations (aspects) between planets at a moment, or between longitudes you supply, with orb, exactness and applying or separating. |
| `resolve_birth_time` | $0.01 | Local time to UTC conversion: convert a local date, time and place into exact Universal Time, with the historical time zone and UTC offset, Delta T, Local Mean Time and true solar time. |

### Calendars and long ranges

| Tool | Price | What it returns |
|---|---|---|
| `astro_event_calendar` | $0.25 | Celestial event calendar (astronomical events): the exact UTC minute and unix timestamp of every eclipse, Mercury retrograde and other station, ingress, new and full moon, planetary aspect, declination parallel and out-of-bounds between two dates, 1800-2200, each with a stable key to join on other data. |
| `planetary_phenomena` | $0.10 | Planetary phenomena between two dates: oppositions, conjunctions with the Sun, greatest elongations of Mercury and Venus, conjunctions between planets, perihelion and aphelion. |
| `local_eclipse` | $0.10 | Solar and lunar eclipses seen from a place: local type, contact times (C1-C4, P1-P4, U1-U4), magnitude, obscuration, duration of totality and the Sun's or Moon's altitude. |
| `seasons` | $0.01 | Equinoxes and solstices of a year, Earth's perihelion and aphelion, and optionally the Sun day by day (sunrise, sunset, day length, declination, equation of time) for a place. |
| `planetary_lines` | $0.20 | Planetary longitude series (angular longitude projections): planet degrees seen from the Earth and from the Sun (geocentric and heliocentric) at a moment or as an hourly-to-weekly time series keyed by unix timestamp, with angular offsets, multi-body longitude averages and an optional linear projection onto a value scale, (degree + 360k) x unit. |
| `sky_analogues` | $0.25 | Sky similarity search (matching skies, planetary cycle analogues): find the past or future dates whose planetary positions best match a reference sky (a moment, now, positions you give, or the sky at a local date-time and place), ranked by a stated angular distance, to line up any dated data with similar skies. |

### Time and coordinates

| Tool | Price | What it returns |
|---|---|---|
| `time_scales` | $0.01 | Julian day, modified Julian day, sidereal time (Greenwich and local, mean and apparent), Delta T, TAI-UTC and UT1-UTC for a moment. |
| `convert_coordinates` | $0.01 | Convert sky coordinates between equatorial (J2000, of date), ecliptic, galactic and horizontal (alt-az) frames. |

### Sky configurations (a place and a moment, compared)

| Tool | Price | What it returns |
|---|---|---|
| `natal_chart` | $0.06 | Sky configuration at a datetime & location (positional snapshot): planets in the ecliptic signs and in houses (11 house systems), ascendant, MC, aspects and a summary. |
| `transits` | $0.08 | Transits to a reference sky configuration (a datetime & location snapshot): the aspects from the sky at a moment, and the exact dates they perfect over a window of up to a year. |
| `synastry` | $0.12 | Pairwise angular geometry between two sky states (datetime & location each): cross-aspects, house overlays both ways and composite midpoints. |

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
| Version | 1.9.0 |
| Transport | Streamable HTTP, stateless |
| Authentication | none required; optional `X-Credits-Token` header |
| Ephemeris | NASA/JPL DE440, 1800-2200 |
| Licence | MIT (this repository); the hosted service has its own [terms](https://astro-agent.dev/terms) |

Support: https://astro-agent.dev/support
