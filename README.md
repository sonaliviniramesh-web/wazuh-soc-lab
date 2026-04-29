# Wazuh SOC Lab – Attack Detection & File Integrity Monitoring

🚨 A hands-on SOC lab built using Wazuh SIEM to simulate real-world attacks and analyze detection mechanisms.

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

---

## 🛠️ Tools & Technologies

* Wazuh SIEM (Manager, Agent, Dashboard)
* Kali Linux (Attack simulation)
* Hydra (Brute-force attack tool)
* VirtualBox (Lab environment)
* Linux (File system monitoring & log generation)

---

## ⚔️ Attack Simulation

### 🔐 1. SSH Brute-Force Attack

A brute-force attack was performed using Hydra against the SSH service:

```bash
hydra -l root -P rockyou.txt ssh://192.168.1.8
```

This generated multiple failed login attempts captured by the Wazuh agent.

![SSH Brute Force](images/04_ssh_bruteforce_attack.png)

---

### 📁 2. File Integrity Monitoring (FIM) Test

To simulate unauthorized changes:

```bash
sudo sh -c 'echo "malicious_entry" >> /etc/passwd'
```

Test file creation and modification:

```bash
sudo touch /etc/fim_test_file
sudo sh -c 'echo "test123" >> /etc/fim_test_file'
```

![FIM Attack Simulation](images/05_fim_attack_simulation.png)

---

## 🚨 Detection & Analysis

### 🔐 SSH Brute-Force Detection

Wazuh detected repeated authentication failures indicating a brute-force attempt.

Key indicators:

* Multiple failed SSH login attempts
* Repeated access attempts from same IP
* `sshd` and `pam` logs

![SSH Detection](images/06_ssh_attack_detected.png)

---

### 📁 File Integrity Monitoring Detection

Wazuh detected:

* File creation (`/etc/fim_test_file`)
* File modification (`/etc/fim_test_file`)
* Critical file modification (`/etc/passwd`)

Detection includes:

* File path
* Timestamp
* Size changes
* Hash changes (MD5, SHA1, SHA256)

![FIM Detection](images/07_fim_detection.png)

---

### 🔍 Event Analysis

Detailed inspection shows:

* Hash changes between old and new file versions
* Metadata differences (size, timestamps)

![Event Analysis](images/08_event_analysis_hash_changes.png)

---

## 🧠 SOC Analysis & Response

### SSH Attack Analysis

* **Source IP:** 192.168.1.8
* **Attack Type:** Brute-force
* **Behavior:** Repeated failed logins

**Impact:**
Potential unauthorized access if credentials are compromised

**Response:**

* Block attacker IP
* Disable root login
* Use SSH keys
* Implement rate limiting

---

### FIM Analysis

* Modified `/etc/passwd`
* Created and modified `/etc/fim_test_file`

**Impact:**
Possible privilege escalation or persistence

**Response:**

* Review file changes
* Restore from backup
* Audit logs
* Restrict file permissions

---

## ⚠️ Challenges Faced

* Agent connection issues due to IP changes
* Network configuration (NAT vs Bridged)
* FIM not triggering due to scan frequency
* Permission issues with protected files

---

## 📊 Conclusion

This project demonstrates how Wazuh can detect real-world attack scenarios in a SOC environment.

It highlights:

* Detection of brute-force attacks
* Monitoring of system integrity
* Importance of log analysis

---

## 🧠 Skills Gained

* SIEM fundamentals and SOC workflow
* Log analysis and threat detection
* File Integrity Monitoring (FIM)
* Incident investigation basics
* Troubleshooting real-world issues

---
