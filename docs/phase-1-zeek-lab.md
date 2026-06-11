# phase-1-zeek-lab

## Dual Mac Mini Zeek Lab

**Goal:** Get hands-on experience with network traffic analysis using real hardware.

```
┌─────────────────────────────────────────┐
│              HOME NETWORK               │
│                                         │
│  ┌──────────────────┐                   │
│  │  2014 Mac Mini   │                   │
│  │  (Primary Node)  │                   │
│  │                  │                   │
│  │  • Zeek          │                   │
│  │  • Wireshark     │                   │
│  │  • PCAP analysis │                   │
│  └──────────────────┘                   │
│                                         │
│  ┌──────────────────┐                   │
│  │  2012 Mac Mini   │                   │
│  │  (Secondary)     │                   │
│  │                  │                   │
│  │  • Zeek          │                   │
│  │  • Test/logging  │                   │
│  └──────────────────┘                   │
│                                         │
│  No centralized logging yet.            │
│  Logs lived on each machine separately. │
└─────────────────────────────────────────┘
```

### What I Did

Both Mac Minis had their spinning hard drives replaced with SSDs before OS installation. Both were clean-installed with Ubuntu 22.04 LTS, and Zeek was installed on each device.

The primary workflow was analyzing ICS/SCADA PCAP samples representing realistic cyberattack scenarios, then using Zeek to generate structured logs and Wireshark to validate packet-level behavior. This gave me:

- Hands-on familiarity with Zeek's log structure (conn.log, dns.log, weird.log, etc.)
- Understanding of how Zeek and Wireshark complement each other: Zeek for metadata and event detection, Wireshark for packet-level inspection
- Practice with Linux CLI fundamentals: network interface configuration, static IP assignment via MAC, navigating and editing config files
- Experience analyzing logs from realistic attack scenarios rather than synthetic lab data

### What This Phase Revealed

The two-machine setup worked, but analyzing logs meant SSH-ing into each machine and grepping through files manually. There was no unified view, no persistence, and no way to correlate events across sources. That limitation was the direct motivation for Phase 2. You can find the original phase 1 write-up below:

---

---

<h1>Home Network Threat Detection Lab with Zeek, Wireshark, and Dual Mac Minis</h1> submitted 2025-08-18

<h2>Summary</h2>

This project documents the setup and configuration of a two-device home cybersecurity lab using a 2012 and 2014 Mac Mini. The goal was to build a functional Zeek and Wireshark logging pipeline capable of analyzing and visualizing network traffic from industrial control system (ICS) PCAPs and simulated malicious data. The project emphasizes hands-on network monitoring, log analysis, and security learning.

<h2>Project Objectives</h2>

* Practice Linux system administration and network interface configuration
* Install and configure Zeek for traffic inspection
* Use Wireshark for packet-level inspection and validation
* Review data from sample attack files (PCAPs)
* Understand log structures and the value of enriched data visibility

<h3>Hardware Used</h3>

Mac Mini (2014)
* Purpose: Primary analysis node
* OS: Ubuntu 22.04 LTS
* Network Role: Hosts Zeek and Wireshark; used for parsing, inspecting, and analyzing network logs
* Upgrades: SSD replacement using iFixit teardown instructions [iFixit Mac Mini 2014 Guide](https://www.ifixit.com/Guide/Mac+mini+Late+2014+Hard+Drive+Replacement/32815)

Mac Mini (2012)
* Purpose: Secondary processing node
* OS: Ubuntu 22.04 LTS (clean install)
* Network Role: Separate testing and log-shipping node
* Upgrades: SSD replacement using iFixit teardown instructions [iFixit Mac Mini 2012 Guide](https://www.ifixit.com/Guide/Mac+mini+Late+2012+Hard+Drive+Replacement/11716)

<h2>Key Configuration Steps</h2>

<h3>Network Configuration</h3>

* Created static DHCP reservations for each Mac Mini on home router 
* Used `ip link` to find network adapter names and MAC addresses
* Practiced using the Linux terminal to move around, change directories, and open config files
* Installed Zeek from source on both machines and added it to the system path

<h3>Zeek</h3>

* Installed Zeek on both machines and added it to the system path
* Created the correct folder structure for logs
* Ran Zeek on ICS/SCADA PCAP samples which simulate real cyberattacks: [PCAPs here](https://github.com/tjcruz-dei/ICS_PCAPS/releases)
* Got log files like `conn.log`, `dns.log`, and `weird.log`

<h3>Wireshark</h3>

* Installed Wireshark to validate packet-level activity seen in the PCAPs
* Used Wireshark in parallel with Zeek logs to trace and correlate packet behavior
* Learned how to trace packets and understand what’s going on in the background

<h2>Skills Gained</h2>

* Static IP assignment via MAC address
* Navigating Linux CLI tools for network diagnostics
* Understanding Zeek’s modular log structure (conn.log, dns.log, weird.log, etc.)
* Analyzing logs from PCAPs representing realistic ICS DDoS scenarios
* Combining Zeek (metadata/event detection) with Wireshark (packet visibility) for multi-layered analysis

<h2>Next Steps</h2>

* Introduce Suricata or Snort for real-time traffic inspection
* Connect a Raspberry Pi as a test device to generate more traffic
* Build dashboards or use other tools to visualize the logs
* Write custom Zeek scripts to trigger detections
