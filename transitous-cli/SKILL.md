---
name: transitous-cli
description: Route planning via transitous CLI — all modes, stdlib only.
version: 0.1.0
metadata:
  hermes:
    tags:
      - Transitous
      - CLI
      - Routing
      - Bus
      - Train
---

# Transitous CLI

Stdlib-only CLI for the Transitous MOTIS API. Plan routes, search stops.
Works from any server (no IP blocks). Install via shared skills repo.

## When to Use

- Any route planning request when `transitous-api` skill is loaded
- "Verbindung von X nach Y"
- "Route planen"
- Stop/station search

## Prerequisites

No API key. No pip packages. Python 3.9+ stdlib only.
The script lives in the shared skills repo at `scripts/transitous`.

## Quick Reference

```bash
# Basic route
python3 scripts/transitous route "Großburgwedel" "Hannover"

# With coordinates
python3 scripts/transitous route --from 52.509,9.858 --to 52.377,9.741

# Flight-only routing
python3 scripts/transitous route "Olbia" "Hannover" --modes AIRPLANE

# Specific date
python3 scripts/transitous route "München" "Berlin" --date 2026-07-04

# Stop search
python3 scripts/transitous stop "Hannover Hbf"
```

## How to Run

Invoke via `terminal` tool from the shared skills directory:

```bash
cd ~/projects/hermes-shared-skills && python3 scripts/transitous route "Großburgwedel" "Hannover"
```

Or symlink into PATH:
```bash
ln -sf ~/projects/hermes-shared-skills/scripts/transitous ~/.local/bin/transitous
transitous route "Hamburg" "Berlin"
```

## Options (route)

| Flag | Default | Description |
|------|---------|-------------|
| `--from`, `--to` | positional args | Origin/destination (name or lat,lon) |
| `--date` | now | ISO 8601 datetime |
| `--modes` | TRANSIT | TRANSIT, RAIL, BUS, AIRPLANE, TRAM, SUBWAY |
| `--num` | 3 | Number of routes |
| `--max-transfers` | 5 | Max changes |
| `--max-time` | 2880 | Max travel time (minutes) |

## Limitations

- Budget airlines (Eurowings, Ryanair) not in GTFS feeds — direct flights may be missing
- No real-time delays (uses static GTFS)
- Stop search uses geocoding — may return multiple IDs for the same station

## Verification

```bash
python3 scripts/transitous route "Großburgwedel" "Hannover" --num 1
```

Expected: 1 route with BUS + SUBWAY, ~50-60 min.
