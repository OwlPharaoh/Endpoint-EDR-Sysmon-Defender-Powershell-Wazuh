# Endpoint Detection & Response (EDR) with Sysmon, Defender, and Wazuh

## Overview
This project demonstrates endpoint detection and response (EDR) techniques using Windows telemetry and Wazuh as the SIEM/XDR platform. The goal was to detect and investigate **real attacker behaviors**, not just malware execution, while preserving native Windows security controls.

The lab mirrors enterprise SOC workflows by correlating:
- Endpoint behavior telemetry (Sysmon)
- Prevention events (Windows Defender)
- SIEM-based detection and correlation (Wazuh)

## Architecture

Windows 11 & Windows Server 2025 (Victims)
  - Sysmon (Olaf Hartong config)
  - Windows Defender
  - Wazuh Agent
    |
Wazuh Manager (v4.14)
  - Detection rules
  - Correlation logic
  - Threat Hunting & Events UI

## Telemetry Sources
- **Sysmon**
  - Process creation (Event ID 1)
  - Command-line context
- **Windows Defender**
  - Blocked execution events
  - Attack Surface Reduction (ASR)
- **PowerShell logging**
  - Script Block Logging (Event ID 4104)
  ![19 enable script block logging and module logging](https://github.com/user-attachments/assets/556caadd-6831-4087-a3f2-f03c1a7a5029)

## Detection Use Cases
- LOLBIN execution (mshta, rundll32, certutil)
- Encoded PowerShell commands
- PowerShell exploitation techniques
- Defender-blocked execution attempts
- Correlation of behavior + prevention signals

## Detection Engineering Approach
- Native Wazuh Sysmon rules were used as the **primary signal**
- Custom rules were **chained or correlated**, not duplicated
- Defender blocks were treated as **high-confidence signals**
- Correlation rules escalated alerts only when multiple independent signals were present

## Correlation Example
A high-severity alert is generated only when:
- Sysmon detects suspicious LOLBIN execution **AND**
- Windows Defender blocks the same behavior


This reduces false positives and reflects real SOC alerting strategy.

## References
- Wazuh Blog: *Detecting PowerShell exploitation techniques in Windows using Wazuh*
- Olaf Hartong Sysmon Modular Configuration

## Screenshots
- Threat Hunting view showing Sysmon alerts
- Defender block events
- Correlated alert (Sysmon + Defender)/ Events UI

![24 log windows defender logs](https://github.com/user-attachments/assets/429fa92c-02b2-402d-8bf1-ac293025dbf4)

![24 rule to detect mshta blocked by windows defender](https://github.com/user-attachments/assets/5065f227-fd82-4de9-94f6-1a195d1e4374)


![25 simulating lolbin attack on windows and dtetecting it in wazuh](https://github.com/user-attachments/assets/3e162888-a785-4c27-8a38-3c5d21383785)

![26 created and validated correlation rules for mshta exe](https://github.com/user-attachments/assets/f570bfb1-22dd-4111-8318-dc74d270c265)


![27 confirmed correlation rules on wazuh](https://github.com/user-attachments/assets/f435e13b-cfbc-43e4-97bf-d5bbc6da6ffd)

---




## Next Project

**Project 03 – Active Directory Attacks & Detection**

