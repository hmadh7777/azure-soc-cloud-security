# 🔐 Azure Cloud Security Operations Platform

> End-to-end cloud-native SOC built on Microsoft Azure — covering Threat Detection, Automated Incident Response, Vulnerability Management, and Zero Trust Identity.

---

## 📌 Project Overview

This project simulates a real enterprise Security Operations Center (SOC) deployed entirely on Microsoft Azure. It was built to demonstrate hands-on skills in cloud security engineering across four core pillars.

| Pillar | Technology Used |
|---|---|
| Threat Detection (SIEM) | Microsoft Sentinel, Log Analytics, KQL |
| Automated Incident Response (SOAR) | Logic Apps, Automation Rules |
| Vulnerability Management | Microsoft Defender for Cloud, Secure Score |
| Zero Trust Identity | Microsoft Entra ID, MFA, RBAC |

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────┐
│                  Azure Subscription                      │
│                                                          │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────────┐  │
│  │  Sentinel   │  │ Defender    │  │   Entra ID      │  │
│  │  SIEM/SOAR  │  │ for Cloud   │  │   Zero Trust    │  │
│  └──────┬──────┘  └──────┬──────┘  └────────┬────────┘  │
│         │                │                   │           │
│  ┌──────▼──────────────────────────────────┐ │           │
│  │     Log Analytics Workspace             │ │           │
│  │     (Central Data Hub)                  │ │           │
│  └──────┬──────────────────────────────────┘ │           │
│         │                                     │           │
│  ┌──────▼──────┐  ┌──────────────┐            │           │
│  │ Honeypot VM │  │  Geo-Attack  │            │           │
│  │ (Real Data) │  │  Map         │            │           │
│  └─────────────┘  └──────────────┘            │           │
└─────────────────────────────────────────────────────────┘
```

---

## 🔧 Components Built

### 1. 🎯 Threat Detection — Microsoft Sentinel
- Deployed Microsoft Sentinel on a dedicated Log Analytics Workspace
- Connected a Windows honeypot VM as a data source via Azure Monitor Agent
- Configured Data Collection Rule to ingest all Windows Security Events
- Built a **geo-attack map workbook** showing real-time attacker IP geolocation

**KQL Detection Rule — Brute Force RDP:**
```kql
SecurityEvent
| where EventID == 4625
| where TimeGenerated > ago(1h)
| summarize FailedAttempts = count() by IpAddress, Account, Computer
| where FailedAttempts > 5
| extend Description = "Possible brute force attack detected"
```

---

### 2. 🤖 Automated Incident Response — Logic Apps SOAR
- Built a Logic Apps playbook triggered by Sentinel alerts
- Created automation rule `Auto-Response-BruteForce` linked to the KQL detection rule
- Playbook automatically adds comments to incidents documenting attacker activity
- Full automation chain: **Attacker hits VM → Sentinel detects → Alert fires → Playbook runs**

---

### 3. 🛡️ Vulnerability Management — Defender for Cloud
- Enabled Microsoft Defender for Cloud (Foundational CSPM — free tier)
- Configured continuous security assessment across the subscription
- Reviewed and documented security recommendations
- Mapped findings to NIST Cybersecurity Framework controls

---

### 4. 🔐 Zero Trust Identity — Microsoft Entra ID
- Enabled per-user MFA for all admin accounts using Microsoft Authenticator
- Assigned **Security Reader** RBAC role following least privilege principles
- Documented Conditional Access policy architecture (requires Entra ID P1)
- Configured identity governance aligned with Zero Trust model

---

## 🍯 Honeypot Setup

A Windows VM was deployed with all inbound ports open to attract real-world attackers:

- **VM Size**: Standard B2s (1 vCPU, 2 GiB RAM)
- **OS**: Windows 10 Pro
- **NSG Rule**: AllowAll inbound (intentional — honeypot design)
- **Purpose**: Capture real brute-force and port-scan attack data
- **Result**: Real attacker IPs captured and visualized on geo-attack map

---

## 📊 Results

| Metric | Value |
|---|---|
| Real attack events captured | ✅ Active |
| KQL detection rules created | 1 (Brute Force RDP) |
| SOAR playbooks deployed | 1 (Auto-Response-BruteForce) |
| Automation rules configured | 1 |
| MFA enabled | ✅ Yes |
| RBAC roles assigned | Security Reader |
| Defender for Cloud | ✅ Enabled (CSPM) |

---

## 🗂️ Repository Structure

```
azure-soc-cloud-security/
│
├── kql/
│   └── brute-force-detection.kql       # KQL detection rule
│
├── playbooks/
│   └── block-ip-playbook.json          # Logic App SOAR playbook
│
├── workbooks/
│   └── geo-attack-map.json             # Sentinel geo-attack map
│
├── screenshots/
│   ├── sentinel-dashboard.png
│   ├── geo-attack-map.png
│   ├── defender-secure-score.png
│   └── rbac-assignments.png
│
└── README.md
```

---

## 🚀 How to Deploy

### Prerequisites
- Azure free account ([azure.microsoft.com/free](https://azure.microsoft.com/free))
- Microsoft Authenticator app (for MFA setup)

### Steps
1. Create Resource Group: `SOC-Lab-RG`
2. Deploy Log Analytics Workspace
3. Enable Microsoft Sentinel on the workspace
4. Deploy Windows VM with open NSG (honeypot)
5. Install Azure Monitor Agent on VM
6. Create Data Collection Rule linking VM to Sentinel
7. Import KQL detection rule from `/kql/`
8. Deploy Logic App playbook from `/playbooks/`
9. Create automation rule in Sentinel
10. Enable Defender for Cloud (Foundational CSPM)
11. Configure Entra ID MFA and RBAC

---

## 📚 Technologies & Tools

![Azure](https://img.shields.io/badge/Azure-0078D4?style=flat&logo=microsoftazure&logoColor=white)
![Microsoft Sentinel](https://img.shields.io/badge/Microsoft_Sentinel-0078D4?style=flat&logo=microsoft&logoColor=white)
![Defender for Cloud](https://img.shields.io/badge/Defender_for_Cloud-00B4D8?style=flat&logo=microsoft&logoColor=white)
![Entra ID](https://img.shields.io/badge/Entra_ID-0078D4?style=flat&logo=microsoftazure&logoColor=white)
![KQL](https://img.shields.io/badge/KQL-Query_Language-blue?style=flat)
![Logic Apps](https://img.shields.io/badge/Logic_Apps-0089D6?style=flat&logo=microsoft&logoColor=white)

- **SIEM**: Microsoft Sentinel
- **SOAR**: Azure Logic Apps
- **Query Language**: KQL (Kusto Query Language)
- **Vulnerability Management**: Microsoft Defender for Cloud
- **Identity**: Microsoft Entra ID (Azure AD)
- **IaC**: ARM Templates
- **Framework**: NIST Cybersecurity Framework

---

## 👤 Author

**Ahmed Bakhadher**
- 📧 Abinali7@gmail.com
- 💼 [LinkedIn](https://linkedin.com/in/ahmed-bakhadher)
- 🌐 [Portfolio](https://hmadh7777.github.io)
---

## 📄 License

This project is open source and available under the [MIT License](LICENSE).
