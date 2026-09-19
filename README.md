# Wazuh SOC Investigation – Endpoint Compromise

![Wazuh](https://img.shields.io/badge/SIEM-Wazuh-blue)
![Focus](https://img.shields.io/badge/Focus-SOC%20Investigation-red)
![Analysis](https://img.shields.io/badge/Analysis-Log%20Correlation-green)
![Status](https://img.shields.io/badge/Project-Completed-success)

## 📌 Overview

This repository documents a practical **SOC Analyst investigation using Wazuh** to investigate suspicious activity on a Windows endpoint.

The assessment focused on identifying a potential malware infection, analyzing endpoint and PowerShell events, investigating outbound network communication, and reconstructing the attack sequence from available logs.

> **Purpose:** Educational / SOC Analyst assessment and portfolio demonstration.

---

## 🎯 Investigation Objectives

The investigation was designed to demonstrate the ability to:

- Identify suspicious files and malware delivery techniques
- Recognize double-extension executable filenames
- Investigate suspicious PowerShell execution
- Identify suspicious command-line parameters
- Investigate outbound network communication
- Correlate file, process, endpoint, and network events
- Build an incident timeline
- Identify the file associated with the initial compromise

---

## 🖥️ Investigation Details

| Field | Details |
|---|---|
| **Primary Tool** | Wazuh |
| **Investigation Type** | SOC / Endpoint Investigation |
| **Endpoint** | `192.168.75.18` |
| **Investigation Date** | 04 September 2026 |
| **Investigation Window** | 15:30 – 16:00 |
| **Subject** | Wazuh / SOC Analyst |
| **Environment** | Windows Endpoint |

---

## 🔎 Key Findings

### 1. Suspicious Double-Extension Executable

The investigation identified:

```text
diwali_offer.ico.exe
```

The filename contains a double-extension pattern, with `.ico` appearing before the actual `.exe` executable extension.

This was identified as the suspicious executable associated with the downloaded archive.

---

### 2. Suspicious PowerShell Execution

The endpoint generated a PowerShell process using:

```text
powershell.exe -nop -w hidden -c
```

The investigation identified:

- `-nop` — PowerShell executed without loading the user profile
- `-w hidden` — PowerShell window configured to remain hidden
- `-c` — command execution mode

The combination was treated as suspicious because it reduces visibility during execution.

---

### 3. Remote Server Communication

The PowerShell process attempted to communicate with the following external server:

```text
14.192.128.13:8080
```

The evidence showed PowerShell using a web request / `DownloadString` mechanism to retrieve additional content.

**Note:** The original assessment evidence contains both HTTP and HTTPS references for this server. The exact protocol should be verified against the underlying Wazuh event before publishing the final sanitized evidence.

---

## 🧩 Attack Timeline

The investigation reconstructed the following sequence:

```text
Phishing Email
      ↓
offer.zip Downloaded
      ↓
ZIP Archive Extracted
      ↓
diwali_offer.ico.exe
      ↓
Executable Launched
      ↓
Hidden PowerShell Execution
      ↓
Remote Server Communication
      ↓
Additional Content Retrieval
```

### Detailed Timeline

| Stage | Observed Activity |
|---|---|
| 1 | User received an email appearing to originate from HR |
| 2 | `offer.zip` was downloaded |
| 3 | The ZIP archive was extracted |
| 4 | `diwali_offer.ico.exe` was executed |
| 5 | PowerShell was launched using `-nop -w hidden -c` |
| 6 | PowerShell contacted `14.192.128.13:8080` |
| 7 | Additional content was requested from the remote server |

---

## 🛡️ SOC Analysis

The investigation demonstrates a common endpoint attack progression involving:

**Initial Delivery → User Execution → PowerShell → Outbound Communication → Payload Retrieval**

The key investigation approach was to correlate multiple Wazuh events using:

- Endpoint IP
- Timestamp
- File activity
- Process activity
- PowerShell command line
- Network-related information

Rather than treating individual alerts in isolation, the events were analyzed as part of a chronological attack chain.

---

## 🧰 Tools & Technologies

### Primary

- **Wazuh**
- **Wazuh Discover**
- Windows endpoint telemetry
- PowerShell event data

### Investigation Techniques

- SIEM log analysis
- Event correlation
- Process investigation
- Malware filename analysis
- PowerShell analysis
- Network event investigation
- Incident timeline reconstruction

---

## 📸 Evidence

Screenshots and investigation evidence can be organized in the following structure:

```text
screenshots/
├── q1-malicious-file.png
├── q2-powershell.png
├── q3-network-communication.png
├── q4-attack-timeline.png
└── q5-initial-compromise.png
```

Each screenshot should correspond to the relevant question and demonstrate the Wazuh event used during the investigation.

---

## 📋 Assessment Questions Covered

### Q1 — Malicious File Identification

**Finding:**

```text
diwali_offer.ico.exe
```

Identified as a suspicious double-extension executable.

### Q2 — Suspicious PowerShell Command

**Finding:**

```text
powershell.exe -nop -w hidden -c
```

Identified as suspicious PowerShell execution using reduced visibility.

### Q3 — Remote Server

**Finding:**

```text
14.192.128.13:8080
```

Identified as the external server contacted during PowerShell execution.

### Q4 — Attack Sequence

The investigation correlated the endpoint, file, process, and network events into a chronological attack chain.

### Q5 — Initial Compromise File

**Finding:**

```text
diwali_offer.ico.exe
```

Identified as the executable associated with the observed compromise.

---

## 🚨 Incident Response Considerations

Based on the observed activity, the assessment concluded that the endpoint should be treated as potentially compromised and appropriate incident-response actions should be considered, including:

1. Isolate the affected endpoint
2. Preserve relevant evidence and logs
3. Investigate the suspicious executable
4. Review PowerShell activity
5. Investigate the external network destination
6. Assess whether additional endpoints were affected
7. Perform remediation and recovery
8. Provide user awareness training regarding suspicious email attachments and files

---

## 📚 Skills Demonstrated

This assessment demonstrates practical experience with:

- 🔍 SOC investigation
- 🖥️ Endpoint monitoring
- 📊 SIEM investigation
- 🧩 Event correlation
- 🦠 Malware identification
- ⚡ PowerShell investigation
- 🌐 Network investigation
- 📝 Incident documentation
- ⏱️ Timeline reconstruction
- 🚨 Initial compromise analysis

---

## 🔐 Security & Privacy

This repository should contain only sanitized assessment material.

Do **not** upload:

- Personal phone numbers
- Personal email addresses
- Passwords
- API keys
- Authentication tokens
- Wazuh credentials
- Private organizational information
- Sensitive production logs

Any screenshots published in this repository should be reviewed and sanitized before being made public.

---

## ⚠️ Disclaimer

This repository represents an authorized educational / assessment investigation.

The analysis is intended for cybersecurity learning, SOC Analyst portfolio demonstration, and defensive security purposes.

The IP addresses, filenames, timestamps, and other indicators shown here are presented as part of the assessment scenario and should not be interpreted as evidence of activity outside that environment.

---

## 👤 Author

**Aditya Senapati**

SOC Analyst / Cybersecurity Enthusiast

Areas of interest:

- Security Operations Center (SOC)
- SIEM
- Wazuh
- Threat Detection
- Incident Response
- Security Automation

---

## ⭐ Portfolio Context

This project is part of a practical cybersecurity portfolio focused on developing hands-on SOC investigation and defensive security skills.

If you find the investigation useful, feel free to explore the other cybersecurity projects in this profile.
