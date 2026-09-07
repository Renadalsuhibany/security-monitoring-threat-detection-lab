# Nmap SYN Scan Detection

## Objective

Detect a controlled **Nmap SYN scan** originating from Kali Linux against the Wazuh Server using Suricata IDS.

## Test Environment

* **Source:** Kali Linux — `192.168.10.5`
* **Destination:** Wazuh Server — `192.168.10.4`
* **Network:** `192.168.10.0/24`
* **Tool:** Nmap
* **IDS:** Suricata
* **Monitoring Platform:** Wazuh

## Test

A TCP SYN scan was performed from Kali Linux against ports 1–1000 on the Wazuh Server:

```bash
sudo nmap -sS -p 1-1000 192.168.10.4
```

The scan identified port `443/tcp` as open on the Wazuh Server.

## Detection Rule

A local Suricata rule was configured to detect SYN traffic from the Kali testing machine to the Wazuh Server:

```text
alert tcp 192.168.10.5 any -> 192.168.10.4 any (flags:S; flow:stateless; msg:"LOCAL Nmap SYN Scan Detected"; sid:1000001; rev:2;)
```

**Signature:** `LOCAL Nmap SYN Scan Detected`
**SID:** `1000001`

## Detection Result

Suricata successfully generated an alert containing:

* Source IP: `192.168.10.5`
* Destination IP: `192.168.10.4`
* Protocol: TCP
* Signature ID: `1000001`
* Signature: `LOCAL Nmap SYN Scan Detected`
* Severity: 3
* Interface: `enp0s3`

The alert was written to:

```text
/var/log/suricata/eve.json
```

Wazuh was configured to monitor this file and successfully ingested the Suricata alert.

The event was visible in Wazuh Discover with Wazuh rule ID `86601`.

## Detection Flow

```text
Kali Linux
    |
    | Nmap SYN Scan
    v
Suricata IDS
    |
    | LOCAL Nmap SYN Scan Detected
    v
eve.json
    |
    v
Wazuh Manager
    |
    v
Wazuh Discover
```

## Evidence

The corresponding Suricata and Wazuh alerts are documented in the project screenshots directory.
