# Scenario: Website Defacement Attack – Incident Handling and Cyber Kill Chain Investigation Using Splunk SIEM Solution


## Overview

A large organisation, Wayne Enterprises, has reported a major security incident involving the defacement of their public-facing website: 
[http://www.imreallynotbatman.com](http://www.imreallynotbatman.com). The attackers left a visible message on the homepage stating, "YOUR SITE HAS BEEN DEFACED," 
confirming a breach and unauthorised access to internal systems.

As part of the organisation's structured **Incident Handling** process, a SOC Analyst is tasked with leading the investigation using **Splunk SIEM**. 
Therefore, the primary objective is to identify the attacker’s path and activities—including **tactics, techniques, and procedures (TTPs)**—uncover compromised assets, 
and map each malicious action to the corresponding stage in the **Cyber Kill Chain** model.


To simulate a realistic enterprise SOC environment, this investigation leverages the **Boss of the SOC (BOTS) Dataset – Version 1**, 
which contains VPN logs, IDS alerts, web access data, system activity logs, and authentication records.

In addition to Cyber Kill Chain mapping, this case study is framed around the **Incident Handling Lifecycle**, which includes:


- 🔹 **Preparation**: Ensuring logging is centralised and accessible through Splunk.
- 🔹 **Detection and Analysis**: Identifying anomalies and suspicious behaviour through correlation searches and threat intelligence.
- 🔹 **Containment, Eradication, and Recovery**: Analysing how the attacker moved through the environment and proposing actions to neutralize the threat.
- 🔹 **Post-Incident Activity**: Documenting findings, lessons learned, and recommendations to harden defenses against similar attacks.

This hands-on case study demonstrates how incident response and threat detection practices converge through SIEM-driven investigation and structured forensic analysis.

## 🔍 Objective
- Investigate how the attacker gained initial access.
- Trace attacker behaviour using Splunk logs and OSINT.
- Map observed actions to the 7 phases of the Cyber Kill Chain:
  1. Reconnaissance
  2. Weaponization
  3. Delivery
  4. Exploitation
  5. Installation
  6. Command & Control (C2)
  7. Actions on Objectives
