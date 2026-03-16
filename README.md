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
4. Leave the VM exposed to the internet to act as a honeypot.

Resource Group
![AZURE-SOC-HoneyPot/RG-SOC-LAB.png](https://github.com/Dleach-hub/Azure-Cloud-SOC-HoneyPot/blob/main/AZURE-SOC-HoneyPot/RG-SOC-LAB.png)

VM Setup
![AZURE-SOC-HoneyPot/VM.png](https://github.com/Dleach-hub/Azure-Cloud-SOC-HoneyPot/blob/main/AZURE-SOC-HoneyPot/VM.png?raw=true)

---

## 2. Configure Log Collection

Security events are collected from the VM using:

* Windows Security Event Logs
* Failed login events
* Authentication activity

These logs are forwarded to **Azure Log Analytics**.

![https://github.com/Dleach-hub/Azure-Cloud-SOC-HoneyPot/blob/main/AZURE-SOC-HoneyPot/SENTINEL_KQL_LOGS.png?raw=true](https://github.com/Dleach-hub/Azure-Cloud-SOC-HoneyPot/blob/main/AZURE-SOC-HoneyPot/SENTINEL_KQL_LOGS.png)

---

## 3. Configure Log Analytics Workspace

1. Create a Log Analytics Workspace
2. Connect the Azure VM to the workspace
3. Enable collection of **Windows Security Events**

LAW
![https://github.com/Dleach-hub/Azure-Cloud-SOC-HoneyPot/blob/main/AZURE-SOC-HoneyPot/LAW-SOC-LAB.png](https://github.com/Dleach-hub/Azure-Cloud-SOC-HoneyPot/blob/main/AZURE-SOC-HoneyPot/LAW-SOC-LAB.png)

---

## 4. Enable Microsoft Sentinel

1. Enable Microsoft Sentinel in the Azure portal
2. Connect it to the Log Analytics Workspace
3. Begin ingesting logs for monitoring and threat detection

Rule configuration
![https://github.com/Dleach-hub/Azure-Cloud-SOC-HoneyPot/blob/main/AZURE-SOC-HoneyPot/SENTINEL_RULES.png](https://github.com/Dleach-hub/Azure-Cloud-SOC-HoneyPot/blob/main/AZURE-SOC-HoneyPot/SENTINEL_RULES.png)

Incidents
![https://github.com/Dleach-hub/Azure-Cloud-SOC-HoneyPot/blob/main/AZURE-SOC-HoneyPot/SENTINEL_INCIDENTS.png](https://github.com/Dleach-hub/Azure-Cloud-SOC-HoneyPot/blob/main/AZURE-SOC-HoneyPot/SENTINEL_INCIDENTS.png)

---

# Global Attack Map Visualization

A Microsoft Sentinel Workbook was created to visualize attacker activity on a global map.

WorkBook with Attachers Location (8 Hrs)
![https://github.com/Dleach-hub/Azure-Cloud-SOC-HoneyPot/blob/main/AZURE-SOC-HoneyPot/IP_ATTACK_MAP.png](https://github.com/Dleach-hub/Azure-Cloud-SOC-HoneyPot/blob/main/AZURE-SOC-HoneyPot/IP_ATTACK_MAP.png)

WatchList
![https://github.com/Dleach-hub/Azure-Cloud-SOC-HoneyPot/blob/main/AZURE-SOC-HoneyPot/MITRE_ATT%26CK.png](https://github.com/Dleach-hub/Azure-Cloud-SOC-HoneyPot/blob/main/AZURE-SOC-HoneyPot/SENTINEL_WATCHLIST.png)

---
# MITRE ATT&CK Mapping

![https://github.com/Dleach-hub/Azure-Cloud-SOC-HoneyPot/blob/main/AZURE-SOC-HoneyPot/MITRE_ATT%26CK%20(2).png](https://github.com/Dleach-hub/Azure-Cloud-SOC-HoneyPot/blob/main/AZURE-SOC-HoneyPot/MITRE_ATT%26CK%20(2).png)

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
