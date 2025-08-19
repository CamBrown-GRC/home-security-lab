# threat-detection-lab-dual-mac-minis
Home Network Threat Detection Lab with Zeek, Wireshark, and Dual Mac Minis

Summary 

This project documents the setup and configuration of a two-device home cybersecurity lab using a 2012 and 2014 Mac Mini. The goal was to build a functional Zeek and Wireshark logging pipeline capable of analyzing and visualizing network traffic from industrial control system (ICS) PCAPs and simulated malicious data. The project emphasizes hands-on network monitoring, log analysis, and security learning.

Hardware Used

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

Project Objectives

* Practice Linux system administration and network interface configuration
* Install and configure Zeek for traffic inspection
* Use Wireshark for packet-level inspection and validation
* Ingest, generate, and review logs from PCAP files simulating real-world ICS attacks
* Understand log structures and the value of enriched data visibility
