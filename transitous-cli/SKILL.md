---
name: transitous-cli
description: Plan public transport routes across 80+ countries, no API key.
version: 0.2.0
metadata:
  hermes:
    tags:
      - Transitous
      - Routing
      - CLI
      - PublicTransport
---

# Transitous CLI

Plan public transport routes by station name or GPS coordinate. Covers 80+
countries with all modes — train, bus, tram, flight, ferry. Uses the free
Transitous MOTIS API (no API key, no IP blocks from datacenter). **Does NOT**
provide real-time delays or cover budget-airline flights not in GTFS feeds.

Stdlib-only Python script — works anywhere with Python 3.9+.

## When to Use

- "Verbindung von Großburgwedel nach Hannover"
- "Wie komme ich von Olbia nach Berlin?"
- "Route von München nach Neapel planen"
- "Flugverbindung von X nach Y"
- Stop/station search by name

## Prerequisites

No API key. No pip packages. Python 3.9+ stdlib only.
The script is installed via shared skills tap at `scripts/transitous`.

## How to Run

Invoke with `terminal` from the tapped repo directory:

```bash
cd ~/projects/hermes-shared-skills && python3 scripts/transitous route "Hamburg" "Berlin"
```

Or symlink into PATH once:

```bash
ln -sf ~/projects/hermes-shared-skills/scripts/transitous ~/.local/bin/transitous
transitous route "München" "Rom"
```

## Quick Reference

```bash
# Route by station name
transitous route "Großburgwedel" "Hannover"

# Route by coordinates
transitous route --from 52.509,9.858 --to 52.377,9.741

# Flight-only mode
transitous route "Olbia" "Hannover" --modes AIRPLANE

# Specific date
transitous route "Berlin" "Paris" --date 2026-07-15

# Stop search
transitous stop "Hannover Hbf"
```

## Procedure

### 1. Route planning

Run `transitous route` with origin and destination. The CLI resolves station
names via the Transitous geocoding API automatically.

```bash
python3 scripts/transitous route "Großburgwedel" "Hannover"
```

For multiple modes, comma-separate:

```bash
python3 scripts/transitous route "Olbia" "Hannover" --modes AIRPLANE,RAIL
```

### 2. Parse output

Output is human-readable text. Each route shows duration, number of changes,
and per-leg detail (mode, departure, arrival, stations).

### 3. Key flags

| Flag | Default | Description |
|------|---------|-------------|
| `--from`, `--to` | positional args | Origin/destination (name or `lat,lon`) |
| `--date` | now | ISO 8601 datetime |
| `--modes` | `TRANSIT` | `RAIL,BUS,AIRPLANE,TRAM,SUBWAY` or `TRANSIT` for all |
| `--num` | 3 | Number of routes to show |
| `--max-transfers` | 5 | Max changes allowed |
| `--max-time` | 2880 | Max total travel time in minutes |

## Pitfalls

- **Budget airlines missing.** Eurowings, Ryanair etc. rarely publish open
  GTFS feeds. Direct flights like OLB→HAJ (Eurowings EW3845) may not appear
  even though they exist in reality. Transitous data ≠ real-world availability.
- **No real-time delays.** Static GTFS only. For DB train delays, add a
  Timetables `/fchg` lookup per station from `db-timetables-api`.
- **Stop IDs are MOTIS-specific** (e.g. `de-DELFI_de:03241:31_G`), not DB EVA
  numbers.
- **Stop names differ between APIs.** "Hannover Hauptbahnhof" (Transitous) vs.
  "Hannover Hbf" (Timetables) — manual name mapping needed for delay lookups.
- **Fair use.** Community-hosted API. Don't poll — ~1 req/s max.

## Verification

```bash
python3 scripts/transitous route "Großburgwedel" "Hannover" --num 1
```

Expected: 1 route with BUS + SUBWAY leg, ~50-60 min travel time.
