# Wazuh SOC Lab – Attack Detection & File Integrity Monitoring

SOC lab using Wazuh SIEM to detect SSH brute-force attacks and file integrity changes.

---

## 📌 Project Overview

This project demonstrates a self-built Security Operations Center (SOC) lab using Wazuh SIEM.
The objective was to simulate real-world attack scenarios and analyze how they are detected using log monitoring and File Integrity Monitoring (FIM).

The lab includes:

* Simulating an SSH brute-force attack using Hydra
* Monitoring system logs through a Wazuh agent
* Detecting unauthorized file modifications
* Analyzing alerts in the Wazuh dashboard

This project focuses on practical attack detection and analysis rather than just tool installation.

---

## 🏗️ Lab Architecture

The lab consists of a simple SOC setup where an endpoint is monitored using a SIEM system.

Kali Linux is used both as an attacker machine and as a monitored endpoint with the Wazuh agent installed.
The agent collects logs and file integrity events and forwards them to the Wazuh manager.

The Wazuh manager processes incoming data, applies detection rules, and generates alerts, which are then visualized in the Wazuh dashboard.

### Architecture Flow

```
[Kali Linux - Attacker]
   - SSH brute-force (Hydra)
   - File modification (/etc/passwd, test files)

        ↓

[Wazuh Agent - Installed on Kali]
   - Log collection
   - File Integrity Monitoring (FIM)

        ↓

[Wazuh Manager - Server]
   - Rule-based detection
   - Event correlation

        ↓

[Wazuh Dashboard]
   - Alert visualization
   - Log analysis
```

---

## 🖥️ Lab Setup

The lab was built using virtual machines:

* **Wazuh Server VM**

  * Runs Wazuh manager, indexer, and dashboard

* **Kali Linux VM**

  * Used for attack simulation and as a monitored endpoint

Both machines were connected using a bridged network to allow direct communication.
