# Security Monitoring & Threat Detection Lab

A hands-on cybersecurity lab focused on **security monitoring, network visibility, and threat detection** using **Wazuh** and **Suricata**.

## Overview

This project demonstrates the implementation of a small Security Operations Center (SOC)-style lab environment for monitoring network activity and detecting suspicious behavior.

The lab uses **Kali Linux** as a controlled testing machine and an **Ubuntu 24.04 Wazuh Server** for centralized security monitoring and alert analysis. **Suricata IDS** monitors network traffic and generates alerts that are collected and analyzed by Wazuh.

## Architecture

The lab is built on an isolated virtual network:

* **Kali Linux** — `192.168.10.5`
* **Wazuh Server** — `192.168.10.4`
* **Virtual Network** — `192.168.10.0/24`
* **Suricata IDS** — monitors traffic on `enp0s3`
* **Wazuh Dashboard / Discover** — security event analysis and visualization

### Detection Flow

```text
Kali Linux
192.168.10.5
      |
      | Test Network Traffic
      v
SOC-LAB Network
192.168.10.0/24
      |
      v
Suricata IDS
      |
      | Alerts
      v
/var/log/suricata/eve.json
      |
      v
Wazuh Manager
      |
      v
Wazuh Dashboard / Discover
```

## Technologies

| Technology   | Purpose                                            |
| ------------ | -------------------------------------------------- |
| Wazuh        | Security monitoring and centralized alert analysis |
| Suricata     | Network IDS and traffic monitoring                 |
| Kali Linux   | Controlled security testing                        |
| Ubuntu 24.04 | Wazuh server operating system                      |
| VirtualBox   | Virtual lab environment                            |
| Nmap         | Network scanning and detection testing             |

## Detection Scenarios

### 1. Network Reconnaissance — Nmap SYN Scan

A controlled **Nmap SYN scan** was performed from Kali Linux against the Wazuh server.

Suricata was configured with a local detection rule:

```text
LOCAL Nmap SYN Scan Detected
SID: 1000001
```

The generated Suricata alert was successfully collected by Wazuh and displayed in Wazuh Discover.

### 2. Suricata Alert Ingestion

Suricata's `eve.json` event log was integrated with Wazuh for centralized monitoring.

Wazuh successfully processed Suricata-generated events, including:

```text
ET INFO Possible Kali Linux hostname in DHCP Request Packet
```

This demonstrates the flow of network security events from **Suricata → Wazuh → Dashboard/Discover**.

## Evidence

Screenshots documenting the lab setup, network connectivity, testing, Suricata alerts, and Wazuh analysis are available in the [`screenshots`](screenshots/) directory.

## Key Skills Demonstrated

* Security Monitoring
* Network Traffic Analysis
* Intrusion Detection Systems (IDS)
* Security Alert Investigation
* Log Collection and Analysis
* Wazuh SIEM/XDR Platform
* Suricata IDS
* Network Reconnaissance Detection
* Nmap
* Linux Administration
* Virtualized Cybersecurity Lab Design

## Project Status

The core monitoring and detection workflow has been implemented and tested using Wazuh and Suricata in an isolated virtual environment.

Future enhancements may include additional endpoints, more detection scenarios, and improved alert tuning to reduce detection noise.
