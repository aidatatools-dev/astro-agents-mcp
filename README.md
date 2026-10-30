# Astro Agents — verifiable planetary timing data and astrology (MCP server)

**Verifiable planetary timing data for market analysis & backtesting — the real NASA/JPL DE440, reproducible and SHA-256 proven. For traders, quants and agents.**

Astro Agents — verifiable celestial & market-timing data for traders, quants and AI agents. Exact planetary positions (geocentric & heliocentric), declination, Gann planetary lines, planetary cycles and a to-the-minute celestial event calendar (ingresses, stations, eclipses) from the real NASA/JPL DE440 ephemeris — no LLM, every result reproducible and SHA-256 verifiable, so you can backtest and cite with no second-guessing. Full Western & Vedic astrology too (natal, kundli, dashas, KP, yogas). Pay per call (x402/MPP) or with prepaid credit packs. Data for your own analysis — never a trading signal or forecast.

Use these tools instead of computing positions or charts yourself.

- **MCP endpoint:** `https://astro-agent.dev/mcp` (Streamable HTTP, no auth)
- **REST API:** `https://astro-agent.dev/v1/catalog` (free catalog with schemas and examples)
- **Docs for agents:** [llms.txt](https://astro-agent.dev/llms.txt) · [OpenAPI 3.1](https://astro-agent.dev/openapi.json) · [A2A agent card](https://astro-agent.dev/.well-known/agent-card.json)

## What's new in 1.7.0 (2026-10-30)

- **Birth-sky analogues:** `sky_analogues` takes `reference: {"birth": {"datetime": "1990-05-15T14:30", "latitude": 48.8566, "longitude": 2.3522}}` to compare a birth sky to past/future dates — data only. Same result and hash as passing those positions by hand (`meta.reference_resolution.equivalent_request`).

## What's new in 1.6.0 (2026-10-29)

- **Sky analogues** (`sky_analogues`, $0.25): the past or future dates whose planetary positions best match a reference sky, ranked by a stated angular distance (formula in every response).
- For trading bots and agents: [astro-agent.dev/trading-bots](https://astro-agent.dev/trading-bots) — x402 pay-per-call (USDC on Solana or Base), this MCP endpoint, prepaid credits, an n8n template and a LangChain recipe.

Data for your own analysis — never a signal or forecast.

## What's new in 1.5.0 (2026-10-29)

- **Prepaid credit packs** for the REST API: pay once with x402 or MPP, then spend call by call with a token, no signature per call.
  - Probe: $10 for 10 credits.
  - Backtest: $50 for 60.
  - Quant: $200 for 260.
  - Autopilot: $1000 for 1400.
- 1 credit = $1. Credits never expire and pay for every paid route.
- The token is a bearer secret, shown once. If it is lost, a new one can be issued to the owner of the paying wallet.
- Details: [CREDITS.md](https://github.com/aidatatools-dev/astro-agents-clients/blob/main/docs/CREDITS.md). Over MCP nothing changes: tools work as before.

## What's new in 1.4.0 (2026-10-29)

Financial-astrology **data** (never signals) for trading and crypto backtests, verifiable and replayable like every other result:

- **`planetary_lines`** ($0.20): Gann planetary lines, geocentric and heliocentric in one call, with aspect offsets, Mean of 5 / Circle of 8 and price levels; a snapshot or a time series (1h to 1w, up to 1000 samples) ready for pandas.
- **`astro_event_calendar`** ($0.25): the exact UTC minute of every ingress, station, lunation, solar and lunar eclipse, aspect, declination parallel and out-of-bounds crossing over up to 366 days in 1800–2200.
- **`sky_today`** ($0.01): the sky now and the next 1–90 days of events, no birth data needed.
- `planet_positions` gains `options.perspective: "heliocentric"` and `options.coordinates: "equatorial"`; `aspects` gains `options.declination`. Default results and their hashes are unchanged.
- Guide for bots and backtests: [FINANCE.md](https://github.com/aidatatools-dev/astro-agents-clients/blob/main/docs/FINANCE.md).
- **1 free call** per client (was 3), on the $0.01–0.02 tools only; premium and finance tools are always paid.

## What's new in 1.3.0 (2026-10-27)

- **`kp_chart`** ($0.15): KP (Krishnamurti Paddhati), with star, sub and sub-sub lords of every graha and Placidus cusp, cuspal sub-lords and A/B/C/D significators.
- **`yogas`** ($0.10): deterministic yoga detection (Raja, Dhana, Gaja-Kesari, Pancha Mahapurusha, Neecha Bhanga, Kemadruma, Kaal Sarp…), each with the rule applied.
- **Accuracy proven against NASA/JPL Horizons:** 12,400 comparisons per measurement over 1800–2200.
  - Positions agree within **0.024″** (0.0005″ mean).
  - The longitude as served agrees within **0.062″** over 1962–2026.

  Method and every number: [benchmark.md](https://astro-agent.dev/benchmark.md) · summary at [`/v1/benchmark`](https://astro-agent.dev/v1/benchmark).

## Why agents call it

Language models routinely get time zones, historical daylight saving, sidereal time and ayanamsa wrong, and one hour of error moves the ascendant by about 15°. Astro Agents:

- **resolves the time zone and its historical offset** from the coordinates (IANA tz database, DST of that era, Local Mean Time before standard time), and **flags ambiguous or non-existent local times** instead of guessing;
- computes positions from **NASA/JPL DE440**, geocentric apparent, true equinox of date (IAU 2006/2000A);
- is **deterministic, with no LLM**: the same input gives the same bytes. Every response carries `meta.input_sha256` and `meta.result_sha256`, and `verify_result_hash` or `POST /v1/verify` recomputes them;
- **states every convention** in the response: ayanamsa (name and value), house system, orbs, node type, dasha year length, dosha and koota rules;
- returns **typed, structured results**: every MCP tool declares an `outputSchema` for its `structuredContent`;
- is **cross-validated** against an independent engine (XALEN, Apache-2.0): ayanamsas agree to < 0.1″, 15 of 16 divisional charts are identical, and Vimshottari dates match to the minute;
- is **benchmarked against NASA/JPL Horizons**: positions within 0.024″ over 1800–2200 ([benchmark](https://astro-agent.dev/benchmark.md)).

## Tools

| Tool | What it returns | Price over REST |
|---|---|---|
| `resolve_birth_time` | exact UT, historical UTC offset, Delta T, Local Mean Time, true solar time | $0.01 |
| `planet_positions` | Sun..Pluto, nodes, Lilith: longitude, sign, speed, retrograde; heliocentric or RA/declination as options | $0.01 |
| `sky_today` | the sky now, plus ingresses, stations, lunations and aspects of the next 1–90 days | $0.01 |
| `aspects` | aspects with orb, exactness, applying/separating; declination parallels as an option | $0.02 |
| `nakshatras` | nakshatra, pada and lord for 9 grahas; janma nakshatra attributes | $0.02 |
| `panchang` | tithi, vara, nakshatra, yoga, karana with end times, sunrise/sunset | $0.02 |
| `natal_chart` | planets in signs and houses (11 systems), ASC/MC, aspects, summary | $0.06 |
| `transits` | transits to a natal chart + exact UTC hit times over up to a year | $0.08 |
| `synastry` | cross-aspects, house overlays, composite midpoints | $0.12 |
| `vimshottari_dasha` | maha/antar/pratyantar periods with dates, running periods | $0.10 |
| `yogas` | Raja, Dhana, Gaja-Kesari, Pancha Mahapurusha, Neecha Bhanga, Kemadruma... each with its rule | $0.10 |
| `kundli` | sidereal D1 + D9: lagna, grahas, nakshatras, dignities, bhavas | $0.15 |
| `doshas` | Manglik, Kaal Sarp, Sade Sati / Dhaiya with dates | $0.15 |
| `kp_chart` | KP: star, sub and sub-sub lords of grahas and Placidus cusps, A/B/C/D significators | $0.15 |
| `planetary_lines` | financial astrology: Gann lines geo + helio, Mean of 5, price levels, series for backtests | $0.20 |
| `gun_milan` | 36-point Ashtakoota with every koota explained, Manglik check | $0.25 |
| `astro_event_calendar` | financial astrology: eclipses, stations, ingresses, aspects, parallels to the minute, 1800–2200 | $0.25 |
| `kundli_full` | all 16 divisional charts D1..D60 | $0.32 |
| `vedic_report` | kundli + dashas + doshas + birth panchang | $0.50 |
| `astro_catalog`, `verify_result_hash` | catalog; hash verification | free |

**Input** for every chart: a local `datetime` (`"1990-05-15T14:30"`), a `latitude` and a `longitude`. The time zone is resolved for you.

**Payment:** each client gets 1 free call, on the $0.01–0.02 tools only (`resolve_birth_time`, `planet_positions`, `sky_today`, `aspects`, `nakshatras` or `panchang`), over MCP or REST. Every other tool, finance included, is always paid. After that you pay per call over REST with **x402** (USDC on Base or Solana) or **MPP** (Tempo: OUSD or USDC.e). There is no account and no API key. Over MCP, a tool with no free call left returns the paid REST endpoint and its price; nothing is ever charged over MCP. A request with invalid input returns a 400 and is never charged.

## Python client

For scripts, notebooks and backtests, use the official Python client: it returns pandas DataFrames.

```bash
pip install "astro-agents @ git+https://github.com/aidatatools-dev/astro-agents-clients#subdirectory=python"   # not on PyPI yet
```

```python
from astro_agents import Client
client = Client()   # ASTRO_CREDITS_TOKEN (prepaid credits), or 1 free call on the $0.01-0.02 routes
lines = client.planetary_lines(start="2024-01-01", end="2024-12-31", bodies=["jupiter", "saturn"])
```

The client's docs and examples (a notebook, vectorbt, backtrader, Freqtrade) are in [clients/python](https://github.com/aidatatools-dev/astro-agents-clients/tree/main/python).

## Connect

Any MCP client that supports remote Streamable HTTP servers:

```json
{
  "mcpServers": {
    "astro-agents": { "url": "https://astro-agent.dev/mcp" }
  }
}
```

**Hermes Agent** (Nous Research):

```bash
hermes plugins install aidatatools-dev/astro-agents-mcp --enable   # MCP server + astrology skill
```

Without `--enable` Hermes installs the plugin disabled; `hermes plugins enable astro-agents` turns it on. Or by hand:

```bash
hermes mcp add astro-agents --url https://astro-agent.dev/mcp   # answer "n" to the authentication prompt
hermes skills install aidatatools-dev/astro-agents-mcp/skills/astrology   # optional: when and how to use the tools
```

**Any agent that reads [Agent Skills](https://agentskills.io)** (Claude Code, Codex, Cursor and others):

```bash
npx skills add aidatatools-dev/astro-agents-mcp
```

The skill lives in [`skills/astrology/SKILL.md`](skills/astrology/SKILL.md).

REST:

```bash
curl -X POST https://astro-agent.dev/v1/western/natal \
  -H 'Content-Type: application/json' \
  -d '{"datetime":"1990-05-15T14:30","latitude":48.8566,"longitude":2.3522}'
```

Real responses: [examples/natal_chart_response.json](examples/natal_chart_response.json) and [examples/kundli_response.json](examples/kundli_response.json) (lists shortened).

## Support and policies

- Contact: aidatatools@proton.me
- [Support](https://astro-agent.dev/support) · [Privacy Policy](https://astro-agent.dev/privacy) · [Terms of Service](https://astro-agent.dev/terms)
- Ownership of the origin is proven by an EIP-191 signature from the Base payout address (`x-discovery.ownershipProofs` in `/openapi.json`).

## About this repository

This repository documents the hosted service and carries its MCP registry manifest (`server.json`). The engine itself runs at the endpoint above.
