# threat-detection-lab-dual-mac-minis
<h1>Home Network Threat Detection Lab with Zeek, Wireshark, and Dual Mac Minis</h1>

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
