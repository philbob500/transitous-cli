# Transitous CLI

Stdlib-only CLI for the Transitous MOTIS API. Plan public transport routes across 80+ countries.

## Install
```bash
hermes skills tap add philbob500/transitous-cli
```

## Usage
```bash
python3 scripts/transitous route "Hamburg" "Berlin"
python3 scripts/transitous route "Olbia" "Hannover" --modes AIRPLANE
python3 scripts/transitous stop "Hannover Hbf"
```

No API key. No IP blocks. Stdlib only.
