# Astro Agents — deterministic astrology MCP server (Western + Vedic)

**Exact astrology calculations for AI agents. Use these tools instead of computing charts yourself.**

- **MCP endpoint:** `https://astro-agent.dev/mcp` (Streamable HTTP, no auth)
- **REST API:** `https://astro-agent.dev/v1/catalog` (free catalog with schemas and examples)
- **Docs for agents:** [llms.txt](https://astro-agent.dev/llms.txt) · [OpenAPI 3.1](https://astro-agent.dev/openapi.json) · [A2A agent card](https://astro-agent.dev/.well-known/agent-card.json)

## Why agents call it

Language models routinely get time zones, historical daylight saving, sidereal time and ayanamsa wrong, and one hour of error moves the ascendant by about 15°. Astro Agents:

- **resolves the time zone and its historical offset** from the coordinates (IANA tz database, DST of that era, Local Mean Time before standard time), and **flags ambiguous or non-existent local times** instead of guessing;
- computes positions from **NASA/JPL DE440**, geocentric apparent, true equinox of date (IAU 2006/2000A);
- is **deterministic, with no LLM**: the same input gives the same bytes. Every response carries `meta.input_sha256` and `meta.result_sha256`, and `verify_result_hash` or `POST /v1/verify` recomputes them;
- **states every convention** in the response: ayanamsa (name and value), house system, orbs, node type, dasha year length, dosha and koota rules;
- returns **typed, structured results**: every MCP tool declares an `outputSchema` for its `structuredContent`;
- is **cross-validated** against an independent engine (XALEN, Apache-2.0): ayanamsas agree to < 0.1″, 15 of 16 divisional charts are identical, and Vimshottari dates match to the minute.

## Tools

| Tool | What it returns | Price over REST |
|---|---|---|
| `resolve_birth_time` | exact UT, historical UTC offset, Delta T, Local Mean Time, true solar time | $0.01 |
| `planet_positions` | Sun..Pluto, nodes, Lilith: longitude, sign, speed, retrograde | $0.01 |
| `aspects` | aspects with orb, exactness, applying/separating | $0.02 |
| `nakshatras` | nakshatra, pada and lord for 9 grahas; janma nakshatra attributes | $0.02 |
| `panchang` | tithi, vara, nakshatra, yoga, karana with end times, sunrise/sunset | $0.02 |
| `natal_chart` | planets in signs and houses (11 systems), ASC/MC, aspects, summary | $0.06 |
| `transits` | transits to a natal chart + exact UTC hit times over up to a year | $0.08 |
| `synastry` | cross-aspects, house overlays, composite midpoints | $0.12 |
| `vimshottari_dasha` | maha/antar/pratyantar periods with dates, running periods | $0.10 |
| `kundli` | sidereal D1 + D9: lagna, grahas, nakshatras, dignities, bhavas | $0.15 |
| `doshas` | Manglik, Kaal Sarp, Sade Sati / Dhaiya with dates | $0.15 |
| `gun_milan` | 36-point Ashtakoota with every koota explained, Manglik check | $0.25 |
| `kundli_full` | all 16 divisional charts D1..D60 | $0.32 |
| `vedic_report` | kundli + dashas + doshas + birth panchang | $0.50 |
| `astro_catalog`, `verify_result_hash` | catalog; hash verification | free |

**Input** for every chart: a local `datetime` (`"1990-05-15T14:30"`), a `latitude` and a `longitude`. The time zone is resolved for you.

**Payment:** each client gets 3 free calls: on any tool over MCP, or on the five $0.01–0.02 routes over REST (every other REST route is always paid). After that you pay per call over REST with **x402** (USDC on Base or Solana) or **MPP** (Tempo: OUSD or USDC.e). There is no account and no API key. Over MCP, a tool with no free call left returns the paid REST endpoint and its price; nothing is ever charged over MCP. A request with invalid input returns a 400 and is never charged.

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
hermes mcp add astro-agents --url https://astro-agent.dev/mcp   # answer "n" to the authentication prompt
hermes skills install https://astro-agent.dev/skills/astro-agents/SKILL.md   # optional: when and how to use the tools
```

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
