# Phase 1: Reconnaissance

##  Objective
To identify signs of attacker reconnaissance activity/attempt targeting the webserver `imreallynotbatman.com` by analysing relevant log sources within the `botsv1` dataset.


##  Approach
Reconnaissance typically involves attackers scanning for open ports, services, or vulnerabilities before launching further attacks. 
Our initial focus is to detect any **inbound communication attempts** aimed at `imreallynotbatman.com`.

From an analyst’s perspective, the first step is to identify the **log source(s)** that contain traffic directed toward the domain. 
Since the dataset includes network-related logs (e.g., web traffic), we can begin by scanning for occurrences of the domain across the index.


## Detail Steps

###   Step 1: Verify Index Availability
- Navigate to **Settings → Indexes** in the Splunk interface
- Confirm that the dataset has been successfully ingested under the index name `botsv1`
- Ensure the data status is active and searchable

###   Step 2: Initial Search for Reconnaissance Activity
Run the following SPL query in the **Search & Reporting** app:

```spl
index=botsv1 imreallynotbatman.com
