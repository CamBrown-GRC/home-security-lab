# Home Security Monitoring Lab

A living documentation of my personal security monitoring infrastructure that I built incrementally over the past year as a hands-on SOC engineering environment using real hardware and real network traffic.

This isn't a tutorial. Rather, it's a record of decisions made, problems solved, and architecture that evolved as my skills and goals grew. The lab currently captures live telemetry from firewall, DNS, network flow, and camera motion events across a multi-machine pipeline and aggregates everything into a single queryable interface.

---

## Table of Contents
- [Why I Built This](#why-i-built-this)
- [Current Architecture](#current-architecture)
- [Skills Demonstrated](#skills-demonstrated)
- [Hardware Inventory](#hardware-inventory)
- [Phase 1 — Dual Mac Mini Zeek Lab](docs/phase-1-zeek-lab.md)
- [Phase 2 — Centralized Log Aggregation Pipeline](docs/phase-2-log-pipeline.md)
- [Phase 3 — Detection Engineering & Dashboards (In Progress)](docs/phase-3-detection-engineering.md)

## Why I Built This

Most home lab guides tell you to spin up a VM, run some sample PCAPs, and call it done. I wanted to do something different: a persistent monitoring environment capturing real traffic from my actual network, with a log pipeline I had to design, deploy, and troubleshoot myself. This had the added effect of contributing to my home network hardening goals.

The longer goal is to build the practical skills that translate to SOC and detection engineering work. This includes not just building familiarity with tools, but understanding the full stack from log generation to ingestion to query and visualization. The architecture has changed significantly as I've learned what works and what doesn't.


## Current Architecture

I set up three dedicated nodes, each with a specific role. All logs flow to a central Loki instance and are queryable through Grafana.

```
┌─────────────────────────────────────────────────────────────────┐
│                        HOME NETWORK                             │
│                                                                 │
│  ┌──────────────────┐      ┌──────────────────┐                │
│  │  2012 Mac Mini   │      │  Raspberry Pi    │                │
│  │  (Zeek Sensor)   │      │  (DNS + Logging) │                │
│  │                  │      │                  │                │
│  │  • Zeek NSM      │      │  • Pi-hole DNS   │                │
│  │  • conn.log      │      │  • DNS query log │                │
│  │  • dns.log       │      │  • Promtail      │                │
│  │  • weird.log     │      │    (binary)      │                │
│  │  • Promtail      │      └────────┬─────────┘                │
│  │    (Docker)      │               │                          │
│  └────────┬─────────┘               │ logs over network        │
│           │                         │                          │
│           └──────────┬──────────────┘                          │
│                      │                                         │
│                      ▼                                         │
│         ┌────────────────────────┐                             │
│         │   2014 Mac Mini        │                             │
│         │   (Central Node)       │                             │
│         │                        │                             │
│         │  • Loki  (log storage) │                             │
│         │  • Grafana  (UI/query) │                             │
│         │  • Promtail (Docker)   │                             │
│         │  • ufw logs            │                             │
│         │  • Frigate NVR        │                             │
│         │  • Plex, Nextcloud,    │                             │
│         │    and other services  │                             │
│         └────────────────────────┘                             │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Log Sources Currently in Pipeline

| Source  | Machine       | Log Type                                                                        | Notes                                                   |
| ------- | ------------- | ------------------------------------------------------------------------------- | ------------------------------------------------------- |
| Zeek    | 2012 Mac Mini | conn.log, dns.log, weird.log, http.log, ssl.log, ssh.log, notice.log, files.log | Network flow + protocol analysis                        |
| Pi-hole | Raspberry Pi  | DNS query log                                                                   | Network-wide DNS visibility                             |
| ufw     | 2014 Mac Mini | Firewall allow/block events                                                     | Host-based firewall telemetry                           |
| Frigate | 2014 Mac Mini | Motion/detection events                                                         | Object detection and camera activity via container logs |


## Skills Demonstrated

| Domain                      | Specific Skills                                                                                 |
| --------------------------- | ----------------------------------------------------------------------------------------------- |
| Network Security Monitoring | Zeek deployment and log analysis, passive traffic capture, protocol inspection                  |
| Log Engineering             | Multi-source log aggregation, Promtail configuration, label design for cross-source correlation |
| SIEM / Query                | LogQL query writing, Loki data source configuration, Grafana dashboard development              |
| Linux Administration        | Ubuntu server configuration, systemd service management, Docker Compose orchestration           |
| Security Architecture       | Network segmentation, IoT isolation, DNS filtering, firewall rule management                    |
| Documentation               | Architecture diagramming, decision documentation, reproducible configuration                    |


## Hardware Inventory

| Device             | Role                                             | OS               | Notes                                   |
| ------------------ | ------------------------------------------------ | ---------------- | --------------------------------------- |
| Mac Mini (2014)    | Central node — Loki, Grafana, Promtail, services | Ubuntu 22.04 LTS | SSD upgrade from original HDD           |
| Mac Mini (2012)    | Zeek sensor                                      | Ubuntu 22.04 LTS | SSD upgrade from original HDD           |
| Raspberry Pi       | Pi-hole DNS + Promtail log shipping              | Raspberry Pi OS  | Promtail runs as native binary  |
| TP-Link Deco       | Mesh network, IoT segmentation                   | —                | ISP modem Wi-Fi disabled                |
| ISP Router | ISP modem (bridge mode)                          | —                | Wi-Fi disabled, Deco handles routing    |
