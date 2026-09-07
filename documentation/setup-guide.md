# Setup Guide

## Lab Overview

This guide documents the setup of the Security Monitoring & Threat Detection Lab.

The lab uses a virtualized environment to simulate a basic security monitoring workflow using Wazuh and Suricata.

## Lab Environment

| Component       | Configuration     |
| --------------- | ----------------- |
| Hypervisor      | VirtualBox        |
| Wazuh Server    | Ubuntu 24.04      |
| Wazuh Server IP | `192.168.10.4`    |
| Testing Machine | Kali Linux        |
| Kali Linux IP   | `192.168.10.5`    |
| Virtual Network | `192.168.10.0/24` |
| Network IDS     | Suricata          |

## Network Configuration

The virtual machines communicate through an isolated VirtualBox NAT Network named `SOC-LAB`.

The main network configuration is:

```text
Network: 192.168.10.0/24

Wazuh Server:
192.168.10.4

Kali Linux:
192.168.10.5
```

Connectivity between the two systems was verified using ICMP and network scanning tests.

## Wazuh Server

The Wazuh platform was installed on Ubuntu 24.04 as an all-in-one deployment.

The installation includes:

* Wazuh Indexer
* Wazuh Manager
* Filebeat
* Wazuh Dashboard

The Wazuh Dashboard was used to analyze and visualize security events.

## Suricata IDS

Suricata was installed on the Wazuh Server and configured to monitor the `enp0s3` network interface.

Suricata generates security events in:

```text
/var/log/suricata/eve.json
```

The Emerging Threats Open rule set was enabled using `suricata-update`.

A custom local detection rule was also created to detect TCP SYN traffic associated with the controlled Nmap scan:

```text
LOCAL Nmap SYN Scan Detected
SID: 1000001
```

## Wazuh and Suricata Integration

Wazuh was configured to monitor the Suricata `eve.json` log file.

This allows Suricata alerts to be collected and analyzed through Wazuh.

The integration flow is:

```text
Network Traffic
      ↓
Suricata IDS
      ↓
/var/log/suricata/eve.json
      ↓
Wazuh Manager
      ↓
Wazuh Dashboard / Discover
```

## Testing

Kali Linux was used as the controlled testing machine.

Network connectivity was verified between Kali Linux and the Wazuh Server.

A controlled Nmap SYN scan was then performed against the Wazuh Server:

```bash
sudo nmap -sS -p 1-1000 192.168.10.4
```

Suricata successfully generated an alert for the custom detection rule, and Wazuh successfully ingested the resulting event.

## Validation

The following components were successfully validated:

* Virtual network connectivity
* Wazuh Server operation
* Suricata IDS operation
* Suricata event generation
* Wazuh ingestion of Suricata events
* Nmap SYN scan detection
* Alert visibility in Wazuh Discover

## Notes

The Windows endpoint and Wazuh Agent are not part of the current implemented lab.

Additional endpoints and detection scenarios can be added as future enhancements.
