<img width="1731" height="1279" alt="Screenshot_16-6-2026_21656_10 0 0 216" src="https://github.com/user-attachments/assets/3c12695e-1b49-484b-9f5d-dba46826bbf7" />
<img width="1731" height="1482" alt="Screenshot_16-6-2026_21548_10 0 0 216" src="https://github.com/user-attachments/assets/b1263aaf-b61f-4a12-9d9d-168bd1d8b7a2" />
<img width="1731" height="1312" alt="Screenshot_16-6-2026_21411_10 0 0 216" src="https://github.com/user-attachments/assets/d5a2c83b-ce4d-4a69-8595-dd54a80c8b98" />
<img width="950" height="445" alt="mitre attack" src="https://github.com/user-attachments/assets/1b902a05-56a8-45e1-8ffc-9229bdedcb2f" />
<img width="944" height="440" alt="malware detection" src="https://github.com/user-attachments/assets/92363ce0-c713-4503-886d-bfbe2b9b80e6" />
<img width="959" height="507" alt="MAIN DASHBOARD" src="https://github.com/user-attachments/assets/b7cbb825-ac3e-49df-9d75-8939e93d944a" />
<img width="959" height="445" alt="agents" src="https://github.com/user-attachments/assets/43e154f4-29ad-4db4-b8d7-750311ab2baf" />
🛡️ Wazuh SIEM Home Lab

A fully functional Security Information and Event Management (SIEM) home lab built using Wazuh, monitoring a live Windows endpoint and detecting real security threats across multiple compliance frameworks.

---

## 📋 Project Overview

This project demonstrates hands-on SOC analyst skills including real-time threat detection, vulnerability management, compliance monitoring, and MITRE ATT&CK mapping — all using enterprise-grade security tools in a home lab environment.

**Tools Used:** Wazuh SIEM v4.9.2 | Ubuntu 24.04 | Windows 11 | VirtualBox

---

## 🏗️ Architecture

```
Windows 11 Endpoint (10.0.0.106)
        |
        | Wazuh Agent v4.9.2
        |
        ▼
Ubuntu VM (10.0.0.216)
├── Wazuh Manager
├── Wazuh Indexer
├── Wazuh Dashboard
└── Filebeat
        |
        ▼
Browser Dashboard (https://10.0.0.216)
```

---

## ⚙️ Setup & Installation

### Prerequisites
- VirtualBox
- Ubuntu 24.04 LTS (50GB disk, 4GB RAM)
- Windows 11 machine

### Wazuh Server Installation (Ubuntu)
```bash
# Download installer
curl -sO https://packages.wazuh.com/4.9/wazuh-install.sh

# Run installation
sudo bash wazuh-install.sh -a -i

# Verify all services running
sudo systemctl status wazuh-manager
sudo systemctl status wazuh-indexer
sudo systemctl status wazuh-dashboard
```

### Windows Agent Installation
```powershell
# Download and install agent
msiexec.exe /i "wazuh-agent-4.9.2-1.msi" /q WAZUH_MANAGER="10.0.0.216" WAZUH_AGENT_NAME="Windows-PC"

# Start the service
NET START WazuhSvc
```

### Agent Registration
```bash
# On Ubuntu — add agent
sudo /var/ossec/bin/manage_agents

# Extract key and import to Windows agent
# Update ossec.conf with Wazuh server IP
# Restart WazuhSvc
```

---

## 🔍 Findings & Analysis

### Active Monitoring Summary
| Metric | Count |
|---|---|
| Total Alerts (24hrs) | 481 |
| Medium Severity Alerts | 265 |
| Low Severity Alerts | 162 |
| Active Endpoints | 1 |
| CVEs Detected | 22 |
| High Severity CVEs | 10 |

---

### 🦠 Malware Detection
Wazuh detected the following threats on the Windows endpoint:

| Alert | Rule ID | Severity | Description |
|---|---|---|---|
| Windows Malware Detected | 513 | Level 9 | Rootcheck identified malware indicators |
| Host-based Anomaly Detection | 510 | Level 7 | Suspicious host behavior detected |
| Emotet Malware Activity | - | Medium | Rootcheck group monitoring |
| Rootkit Activity | - | Medium | Trojaned file version detected |

**Analysis:** These alerts were triggered by Wazuh's rootcheck module scanning the Windows endpoint. The detections indicate suspicious system configurations and potential malware indicators consistent with known Windows malware families. Further investigation revealed these were primarily false positives from system processes, but the detection capability is confirmed operational.

---

### 🗺️ MITRE ATT&CK Mapping
Wazuh automatically mapped detected events to MITRE ATT&CK techniques:

| Tactic | Technique | Agent |
|---|---|---|
| Defense Evasion | Disable or Modify Tools | Windows-PC |
| Initial Access | Valid Accounts | Windows-PC |
| Persistence | Windows Service | Windows-PC |
| Privilege Escalation | Valid Accounts | Windows-PC |

**Key Insight:** The most frequent tactic detected was **Defense Evasion**, indicating Windows system processes attempting to modify security tools — a common technique used by attackers to avoid detection.

---

### 🔒 Vulnerability Management
Wazuh scanned the Windows endpoint and identified the following CVEs:

| Severity | Count | Top Vulnerable Package |
|---|---|---|
| High | 10 | Python 3.11.0 (10 CVEs) |
| Medium | 12 | urllib3, Jinja2, requests |
| Critical | 0 | - |

**Top CVEs Identified:**
- CVE-2007-4559
- CVE-2020-10735
- CVE-2021-28861
- CVE-2022-40896
- CVE-2022-45061

**Remediation Recommendation:** Update Python to latest version and patch urllib3 and Jinja2 libraries to address high severity vulnerabilities.

---

### 📊 Compliance Framework Monitoring

Wazuh monitored compliance across multiple frameworks simultaneously:

#### NIST 800-53
- **Total Alerts:** 462
- **Max Rule Level:** 7
- **Top Controls:** CM.1, AU.14, SC.8, AC.7, AU.6

#### PCI DSS
- **Top Requirements:** 2.2, 2.2.5, 4.1, 10.6.1, 7.1
- **Primary Agent:** Windows-PC generating majority of alerts

#### Additional Frameworks
- HIPAA monitoring active
- GDPR monitoring active
- TSC monitoring active

---

## 🎯 SOC Analyst Skills Demonstrated

| Skill | Evidence |
|---|---|
| SIEM Deployment | Deployed Wazuh on Ubuntu with all components |
| Endpoint Monitoring | Connected Windows 11 agent, generating live events |
| Alert Triage | Analyzed 481+ alerts across severity levels |
| Malware Detection | Identified Emotet and rootkit indicators |
| Vulnerability Management | Identified 22 CVEs with CVSS scoring |
| MITRE ATT&CK Mapping | Mapped alerts to Defense Evasion, Persistence, Initial Access |
| Compliance Monitoring | Monitored NIST 800-53, PCI DSS, HIPAA, GDPR simultaneously |
| Incident Investigation | Investigated malware alerts and determined false positive vs true positive |

---

## 📚 Key Learnings

1. **Real alerts are noisy** — 481 alerts in 24 hours from a single Windows endpoint demonstrates why alert triage is a critical SOC skill
2. **MITRE ATT&CK is essential** — Mapping alerts to tactics helps understand attacker intent beyond just individual events
3. **Compliance and security overlap** — The same events trigger both security alerts and compliance violations across multiple frameworks
4. **Vulnerability management is ongoing** — 22 CVEs on a standard Windows machine shows why patch management is critical

---

## 🔮 Next Steps

- [ ] Simulate brute force attack and detect with Wazuh
- [ ] Connect Ubuntu VM as second monitored endpoint
- [ ] Build Python script to fetch and analyze Wazuh alerts via API
- [ ] Integrate Claude AI API for automated alert analysis and incident reports
- [ ] Set up custom detection rules for specific attack scenarios
- [ ] Document full incident response workflow

---

## 📁 Repository Structure

```
wazuh-siem-lab/
├── README.md
├── screenshots/
│   ├── 01-overview-dashboard.png
│   ├── 02-windows-agent-active.png
│   ├── 03-malware-detection.png
│   ├── 04-mitre-attack-mapping.png
│   ├── 05-vulnerability-detection.png
│   ├── 06-threat-hunting.png
│   └── 07-nist-800-53-compliance.png
└── docs/
    └── setup-guide.md
```

---

## 👤 Author

**Vaidehi Thombare**
- MS Cybersecurity — Northeastern University
- CEH | ISO 27001 Lead Auditor
- [LinkedIn](https://www.linkedin.com/in/vaidehithombare/)
- [GitHub](https://github.com/Vaidehit06)

---

## ⚠️ Disclaimer

This lab is built for educational purposes only. All testing was performed on personally owned equipment in an isolated home lab environment.
