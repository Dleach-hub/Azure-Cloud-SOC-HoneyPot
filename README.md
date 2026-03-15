# Azure Cloud SOC Honeypot

## Overview

The **Azure Cloud SOC Honeypot** project simulates a real-world **Security Operations Center (SOC)** environment using Microsoft Azure. A publicly exposed Windows Virtual Machine is deployed as a honeypot to intentionally attract malicious activity from the internet. Attack attempts such as brute-force login attempts are captured, logged, and analyzed using Azure-native security tools.

Security telemetry is collected through **Windows Event Logs**, centralized in **Azure Log Analytics**, and monitored using **Microsoft Sentinel**, a cloud-native SIEM platform. The project also enriches attacker IP addresses with geolocation data and visualizes global attack activity using a **Sentinel workbook attack map**.

This project demonstrates practical experience in **cloud security monitoring, threat detection engineering, SIEM configuration, and security event analysis**.

---

# Architecture

The security monitoring pipeline follows this workflow:

```
Internet Attackers
        ↓
Public Azure Virtual Machine (Honeypot)
        ↓
Windows Security Event Logs
        ↓
Azure Log Analytics Workspace
        ↓
Geolocation Enrichment
        ↓
Microsoft Sentinel (SIEM)
        ↓
Threat Detection, Dashboards, and Investigation
```

### Components

**Azure Virtual Machine**

* Public-facing Windows VM acting as the honeypot
* Exposed to the internet to attract attack traffic
* Logs authentication activity and security events

**Network Security Group**

* Allows inbound traffic to simulate a vulnerable target

**Azure Log Analytics Workspace**

* Central log repository
* Stores and processes Windows security logs

**Microsoft Sentinel**

* Cloud SIEM used for monitoring and investigation
* Supports queries, dashboards, and detection rules

---

# Project Objectives

* Simulate SOC monitoring in a cloud environment
* Capture real-world attacker behavior
* Centralize security logs for analysis
* Detect brute-force authentication attempts
* Visualize global attack activity
* Develop threat detection queries using KQL

---

# Environment Setup

## 1. Deploy Azure Virtual Machine

1. Configure Resource group
2. Create a Windows Virtual Machine in RG
3. Allow inbound RDP traffic In NSG
4. Leave the VM exposed to the internet to act as a honeypot

---

## 2. Configure Log Collection

Security events are collected from the VM using:

* Windows Security Event Logs
* Failed login events
* Authentication activity

These logs are forwarded to **Azure Log Analytics**.

---

## 3. Configure Log Analytics Workspace

1. Create a Log Analytics Workspace
2. Connect the Azure VM to the workspace
3. Enable collection of **Windows Security Events**

---

## 4. Enable Microsoft Sentinel

1. Enable Microsoft Sentinel in the Azure portal
2. Connect it to the Log Analytics Workspace
3. Begin ingesting logs for monitoring and threat detection

---

# Geolocation Enrichment

To better understand attacker activity, captured IP addresses were enriched with **geolocation data**. This converts raw IP addresses into geographic information including:

* Country
* City
* Latitude
* Longitude

This enrichment allows security events to be visualized geographically and helps identify which regions generate the most attack traffic.

Example query used to identify attacking IP addresses:

```kql
SecurityEvent
| where EventID == 4625
| where IpAddress != "-"
| summarize FailedAttempts = count() by IpAddress
| order by FailedAttempts desc
```

The enriched data is then used for visualization in Sentinel dashboards.

---

# Advanced Sentinel Detection Rules

Custom **Microsoft Sentinel analytics rules** were created to detect suspicious activity and simulate real SOC alerting.

## RDP Brute Force Detection

Detects repeated login failures from the same IP address within a short time period.

```kql
SecurityEvent
| where EventID == 4625
| summarize AttemptCount = count() by IpAddress, bin(TimeGenerated, 5m)
| where AttemptCount > 10
```

**Purpose**

* Detect brute-force password attacks
* Alert security analysts to suspicious activity
* Identify automated attack sources

---

## Suspicious Authentication Activity

Highlights accounts receiving large numbers of failed login attempts.

```kql
SecurityEvent
| where EventID == 4625
| summarize FailedAttempts = count() by Account, IpAddress
| where FailedAttempts > 20
```

**Purpose**

* Identify targeted accounts
* Detect credential-stuffing attempts
* Highlight high-risk authentication behavior

---

# Global Attack Map Visualization

A Microsoft Sentinel Workbook was created to visualize attacker activity on a global map.

---
# MITRE ATT&CK Mapping

Attacker behavior is mapped to the **MITRE ATT&CK** framework to standardize detections.

| Observed Behavior                      | MITRE Tactic      | Technique                         | ID    |
| -------------------------------------- | ----------------- | --------------------------------- | ----- |
| RDP Brute Force                        | Credential Access | Brute Force                       | T1110 |
| Targeting RDP Service                  | Lateral Movement  | Remote Services                   | T1021 |
| Initial Public-Facing Exploit Attempts | Initial Access    | Exploit Public-Facing Application | T1190 |

Mapping attacks to MITRE ATT&CK helps align the project with **industry best practices** and demonstrates a professional SOC workflow.

---

# Custom Sentinel Workbook

A **custom Sentinel workbook** aggregates logs, geolocation data, and detection results to provide a centralized SOC dashboard.

### Features

* 🌍 Global attack map
* 📊 Failed login attempts over time
* 🌐 Top attacking IP addresses
* 🗺 Top attacking countries
* 🔑 Most targeted user accounts


---

# Security Monitoring Workflow

```
Internet Attackers
        ↓
Azure Honeypot VM
        ↓
Windows Security Logs
        ↓
Azure Log Analytics
        ↓
Geolocation Enrichment
        ↓
Microsoft Sentinel
        ↓
MITRE ATT&CK Mapping & Custom Workbook
```

---

# Skills Demonstrated

* Cloud security monitoring
* SIEM deployment and management
* Threat detection engineering
* Kusto Query Language (KQL)
* MITRE ATT&CK mapping
* Security dashboard development
* Incident investigation workflows

---
