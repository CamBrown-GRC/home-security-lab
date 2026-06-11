# phase-2-log-pipeline

# Centralized Log Aggregation Pipeline

**Goal:** Build a log ingestion pipeline that aggregates telemetry from multiple sources into a single queryable interface.

:pushpin: This is the current state of the lab.

---
## Architecture Decisions

***Zeek moves to the 2012 Mac Mini exclusively***: The 2014 Mini was already running a significant Docker workload. This allowed for a dedicated sensor and dedicated aggregation node, leading to cleaner architecture and resource contention.

***Raspberry Pi joins the pipeline***: As I was already running Pi-hole as the network-wide DNS resolver, the Pi became a natural log source. Promtail runs as a native binary on the Pi (rather than Docker) given its lower resources and single-purpose role.

***Promtail as the log shipper everywhere else***: Runs as a Docker container on both Mac Minis, watching specific log paths and forwarding new entries to Loki with labels attached (job, host, log type). These labels are what make cross-source correlation possible in LogQL.

***Loki + Grafana on the 2014 Mini***: Loki stores and indexes logs by label. Grafana connects to Loki as a data source and provides the query interface, dashboards, and (eventually) alerting. 

## Why Loki Instead of ELK Stack

The original plan was to use Elasticsearch + Kibana (the classic SIEM stack). After looking at the memory requirements against the existing workload on the 2014 Mac Mini (which was already running Plex, Nextcloud, Frigate, and several other Docker services), Elasticsearch was off the table. Loki indexes only metadata labels rather than full log text, which makes it significantly lighter. Grafana serves as the frontend for both Loki and any other metrics sources, so there's no separate Kibana instance needed.

## What This Unlocks

With everything flowing into Loki, Grafana becomes a single pane of glass across all log sources. A LogQL query like:

```
{job="zeek"} |= "192.168.70.40"
```

returns all Zeek events involving that IP. Combine it with a Pi-hole query for the same IP and a ufw query and you have multi-source correlation that would otherwise require manual grepping across three machines.
