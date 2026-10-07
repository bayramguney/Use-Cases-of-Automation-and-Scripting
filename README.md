# Use-Cases-of-Automation-and-Scripting

# 🤖 Assisted Lab: Use Cases of Automation and Scripting

> **Threat Intelligence Automation | Bash Scripting | Linux Security | Security+ (SY0-701)**

## 📌 Overview

This lab demonstrates how security automation can improve threat detection and incident response. Using **Kali Linux**, automated Bash scripts consume **threat intelligence feeds** to update firewall rules and remove malicious files from the system.

The lab consists of two automation scenarios:

- Automatically blocking malicious IP addresses using **iptables**
- Automatically detecting and removing malicious files using **hash-based verification**

---

# 🎯 Objectives

- Automate firewall rule creation
- Process threat intelligence feeds
- Schedule security tasks with Cron
- Create Bash automation scripts
- Detect malicious files using cryptographic hashes
- Remove malicious files automatically
- Improve operational efficiency through security automation

---

# 🖥️ Lab Environment

| Machine | Operating System | Purpose |
|---------|-----------------|----------|
| **KALI** | Kali Linux | Security automation and scripting |

---

# 🛠️ Tools Used

- Bash
- iptables
- curl
- cron (crontab)
- awk
- vim
- find
- chmod
- sha256sum
- grep
- Linux Terminal

---

# 🔐 Security Concepts

- Security Automation
- Threat Intelligence
- Threat Feeds
- Indicators of Compromise (IoCs)
- Malware Detection
- Firewall Automation
- Scheduled Tasks
- Bash Scripting
- Vulnerability Management
- Security Orchestration

---

# Lab 1 – Automating Firewall Rules

## Scenario

A threat intelligence feed provides a list of known malicious IP address ranges. Instead of manually creating firewall rules, a Bash script automates the process by retrieving the feed and adding **iptables** rules to block malicious traffic.

---

## View the Threat Feed

```bash
curl 127.0.0.1/evil_IP.feed
```

The simulated feed contains malicious IP ranges collected from threat intelligence sources.

---

## Manually Block an IP Address

Example:

```bash
iptables -A INPUT -s 5.134.128.0/19 -j DROP
```

While effective, manually creating rules for every malicious IP is inefficient.

---

## Automate Rule Creation

The provided **ip_block.sh** script:

- Downloads the threat feed
- Extracts IP addresses
- Creates DROP rules automatically
- Updates the firewall

Run the script:

```bash
./ip_block.sh
```

---

## Schedule Automatic Execution

Configure Cron to run the script every day at **1:00 AM**.

```bash
echo "0 1 * * * /bin/bash /root/ip_block.sh" | crontab -
```

---

## Remove Duplicate Firewall Rules

Repeated execution creates duplicate firewall entries.

The script was improved by adding:

```bash
iptables-save | awk '!seen[$0]++' | iptables-restore
```

This exports the rules, removes duplicates, and restores the cleaned rule set.

---

# Lab 2 – Automating Malware Removal

## Scenario

A second threat intelligence feed contains known malware filenames and their SHA-256 hashes.

The provided Bash script:

- Downloads the malware feed
- Searches the filesystem
- Compares file hashes
- Deletes verified malicious files

This prevents accidental deletion of files that simply have the same filename.

---

## View the Malware Feed

```bash
curl 127.0.0.1/mal_hash.feed
```

---

## Malware Detection Logic

The script performs the following:

1. Download the threat feed
2. Read malware filenames
3. Search the system
4. Calculate SHA-256 hashes
5. Compare hashes with the threat feed
6. Remove matching files

Only files matching **both filename and SHA-256 hash** are deleted.

---

## Optimize the Scan

Originally the script searched the entire filesystem.

For testing purposes, it was modified from:

```bash
find /
```

to:

```bash
find /usr/share
```

This greatly reduced scan time.

---

## Make the Script Executable

```bash
chmod +x remove-malware.sh
```

---

## Run the Malware Removal Script

```bash
./remove-malware.sh
```

---

## Malware Removed

The script successfully detected and removed:

- ✅ `nc.exe`
- ✅ `klogger.exe`

These files matched both the filename and SHA-256 hash from the threat intelligence feed.

The following files were **not removed** because their hashes did not match:

- `vncviewer.exe`
- `FuzzDB.nc.exe`
- `sqlninja/apps/nc.exe`

---

## Schedule Automatic Malware Scanning

Run the malware removal script every day at **2:00 AM**.

```bash
echo "0 2 * * * /bin/bash /root/remove-malware.sh" | crontab -
```

---

# Skills Demonstrated

- Linux Administration
- Bash Scripting
- Firewall Management
- iptables Configuration
- Threat Intelligence Integration
- Malware Detection
- SHA-256 Verification
- Scheduled Automation
- Cron Job Management
- Threat Hunting
- Secure Operations
- Security Automation

---

# Key Takeaways

- Threat intelligence feeds can be integrated into automated security workflows.
- Bash scripts can dynamically update firewall rules using malicious IP feeds.
- Cron enables scheduled execution of security tasks without manual intervention.
- Hash verification provides a reliable method for identifying malicious files.
- Matching both the filename and SHA-256 hash reduces false positives during malware removal.
- Removing duplicate firewall rules keeps rule sets efficient and easier to manage.
- Security automation improves response time, consistency, and operational efficiency while reducing manual effort.

---

# 🏷️ GitHub Topics

```text
security-automation
bash
linux
kali-linux
iptables
threat-intelligence
threat-feeds
malware-analysis
sha256
cron
crontab
automation
incident-response
soc-analyst
threat-hunting
cybersecurity
securityplus
blue-team
linux-security
github-lab
```
