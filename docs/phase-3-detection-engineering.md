## Phase 3 — Detection Engineering & Dashboards (Status: In Progress)

**Goal:** Move from passive log collection to active detection logic and operational visibility.

### Planned Work

**Grafana dashboards** for real-time visibility:

- Firewall blocks per minute with source IP breakdown
- Top DNS queries and blocked domains from Pi-hole
- Zeek connection volume by destination port and protocol
- Frigate camera activity visualization

**Detection logic in LogQL** — alert rules targeting:

- Port scan signatures (high conn.log volume from single source IP)
- Unexpected outbound connections to unusual destination ports
- DNS-based indicators (queries to known malicious domains, excessive NXDOMAIN responses)
- Brute force patterns in authentication logs

**Config version control** — Docker Compose files, Promtail configs, and Grafana dashboard JSON exports committed here for reproducibility.
