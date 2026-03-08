---
description: Real-time Toronto transit — bus & streetcar arrivals, vehicle tracking, alerts, stop search
allowed-tools: Bash, Read
name: ttc
version: 0.1.3
metadata:
  openclaw:
    requires:
      bins:
        - ttc
    install:
      - kind: node
        package: "@lucasygu/ttc"
        bins: [ttc]
    os: [macos, linux, windows]
    homepage: https://github.com/lucasygu/ttc-cli
tags:
  - ttc
  - toronto
  - transit
  - gtfs
  - bus
  - streetcar
  - realtime
  - productivity
---

# TTC CLI — Toronto Transit Commission

Real-time bus and streetcar tracking for Toronto. Next arrivals, vehicle positions, service alerts, and stop search — all from the terminal.

## Prerequisites

- Node.js 22+
- No authentication required — all feeds are public

## Quick Reference

```bash
ttc next "king spadina"           # Next arrivals at a stop
ttc next 8126                     # By stop code
ttc route 504                     # Route info + active vehicles
ttc vehicles 504                  # Live positions on a route
ttc alerts                        # Service alerts
ttc alerts --broad                # Include subway alerts
ttc nearby                        # Auto-detect location (macOS)
ttc nearby 43.6426,-79.4002       # Or provide coordinates
ttc stops 504                     # Active stops on a route
ttc routes                        # List all surface routes
ttc routes --type streetcar       # Filter by type
ttc search "broadview station"    # Fuzzy stop search
ttc status                        # System overview
```

## Commands

### `ttc next <stop>`
Show next arrivals at a stop. Accepts stop name (fuzzy matched), stop ID, or stop code.

### `ttc route <number>`
Show route info: type (bus/streetcar), directions/headsigns, active vehicle count, alerts.

### `ttc vehicles [route]`
Live vehicle positions. Shows fleet number, route, status, current stop, and occupancy.

### `ttc alerts [route]`
Service disruptions and alerts. Use `--broad` for subway alerts too.

### `ttc nearby [lat,lng]`
Find nearest stops and their upcoming arrivals. Default 500m radius. On macOS, auto-detects your location if no coordinates are provided.

### `ttc routes`
List all surface routes. Filter with `--type bus` or `--type streetcar`.

### `ttc search <query>`
Fuzzy search for stops by name. Strips noise words (St, Ave, At) for better matching.

### `ttc stops <route>`
List currently active stops on a route (derived from live vehicle and prediction data).

### `ttc status`
System overview: active vehicles, active routes, alert count, static data freshness.

## Global Options

All commands support:
- `--json` — Output as JSON for agent/script consumption

## Live Monitoring with `/loop`

Combine with Claude Code's `/loop` command for real-time monitoring:

```bash
/loop 3m /ttc next "king spadina"    # Watch arrivals while getting ready
/loop 5m /ttc alerts                 # Monitor disruptions during storms
/loop 2m /ttc vehicles 504           # Track vehicles approaching your stop
/loop 3m /ttc nearby                 # Refresh nearby arrivals as you walk
```

## Data Sources

- **Real-time**: GTFS-RT protobuf feeds from `bustime.ttc.ca` (no auth)
- **Static**: Pre-bundled stop/route/trip data from Open Toronto GTFS
- **Coverage**: Surface transit only (buses + streetcars). Subway alerts available via `--broad`.
