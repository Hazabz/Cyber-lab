# Scenario: Website Defacement Attack – Incident Handling and Cyber Kill Chain Investigation Using Splunk SIEM Solution


## Overview

A large organisation, Wayne Enterprises, experienced a cyberattack that led to the defacement of their public-facing website. 
The site now shows a message from the attackers stating, "YOUR SITE HAS BEEN DEFACED."

As a SOC Analyst, you are tasked with investigating the cyberattack using **Splunk** SIEM solution and mapping each of the attacker's activities/steps to the **Cyber Kill Chain** model.

To simulate a realistic environment, the investigation uses dataset from the **Boss of the SOC (BOTS) Dataset – Version 1**, a publicly available dataset provided by Splunk.
This dataset includes various logs such as VPN connections, IDS alerts, authentication attempts, and system activity—enabling comprehensive tracking of attacker behaviours 
across multiple stages of the attack.


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
