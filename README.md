# amber-tracker-db

Public database of web tracking tactics cataloged by the [amber](https://github.com/spacedudem/amber) browser extension.

## What this is

Every time amber captures a web page, it strips tracking scripts and classifies each one using Gemini Nano (Chrome's built-in on-device AI). Classifications are contributed anonymously to this repository. The result is a growing, machine-readable catalog of how tracking code is actually implemented — not just which domains serve trackers, but the specific tactic type, code signatures, and behavioral patterns of each snippet.

This fills a gap in the privacy tooling ecosystem. Domain-based blocklists (EasyList, Disconnect, Pi-hole) identify tracker domains but cannot block first-party tracking embedded inline or detect novel implementations not yet associated with known domains. amber's tactic-level data enables a new class of behavioral blocking rules.

## Data format

`tracker_tactics.json` is the primary database. Each entry in `tactics` describes one classified snippet:

- `id` — unique identifier
- `tactic_type` — classification (e.g. `fingerprinting`, `session_replay`, `beacon`, `pixel`, `keylogger`, `ad_network`, `analytics`)
- `source_domain` — domain where the snippet was found (no path or query string)
- `snippet_hash` — SHA-256 of the raw stripped code (code itself is not stored)
- `classification_confidence` — Gemini Nano confidence score 0.0–1.0
- `first_seen` — ISO 8601 timestamp of first observation
- `occurrence_count` — how many times this hash has been submitted

## Exports

The `exports/` directory contains derived blocklist formats regenerated on each database update:

| File | Format | Compatible with |
|---|---|---|
| `ublock_filters.txt` | uBlock Origin filter syntax | uBlock Origin, Adblock Plus |
| `hosts.txt` | HOSTS file format | system hosts, dnsmasq |
| `pihole.txt` | Pi-hole gravity list format | Pi-hole |
| `disconnect.json` | Disconnect.me JSON schema | Firefox Tracking Protection |

## Contributing

Contributions flow automatically through the amber extension when users opt in to anonymous tactic sharing. Manual contributions and corrections can be submitted via pull request. See the amber [Agent Playbook](https://github.com/spacedudem/amber/blob/main/docs/AGENT_PLAYBOOK.md) for the automated contribution pipeline.

## License

Database entries: [CC0 1.0 Universal](https://creativecommons.org/publicdomain/zero/1.0/) — no rights reserved.  
Export format code: MIT.
