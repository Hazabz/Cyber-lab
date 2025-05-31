# Phase 1: Reconnaissance

##  Objective
To identify signs of attacker reconnaissance activity/attempt targeting the webserver `imreallynotbatman.com` by analysing relevant log sources within the `botsv1` dataset using the Splunk SIEM platform.

This phase of the investigation aims to:
- Detect and trace early probing attempts directed at the web server
- Identify the external IP address responsible for scanning activity
- Discover the web server's IP address based on incoming traffic
- Determine the **Content Management System (CMS)** used by the web application, which may have been fingerprinted by the attacker
- Identify the **automated web scanner or tool** leveraged by the attacker during the reconnaissance phase
- Analyse Suricata alerts and extract any associated **Common Vulnerabilities and Exposures (CVE)** identifiers triggered during the reconnaissance attempts

These goals reflect a foundational step in the incident response process, helping to attribute early attacker behaviors, assess exposure, and prepare for subsequent stages in the Cyber Kill Chain.


##  Approach
Reconnaissance typically involves attackers scanning for open ports, services, or vulnerabilities before launching further attacks. 
Our initial focus is to detect any **inbound communication attempts** aimed at `imreallynotbatman.com`.

From an analyst’s perspective, the first step is to identify the **log source(s)** that contain traffic directed toward the domain. 
Since the dataset includes network-related logs (e.g., web traffic), we can begin by scanning for occurrences of the domain across the index.


## Detail Steps

###   Step 1: Verify index availability
- Navigate to **Settings → Indexes** in the Splunk interface
- Confirm that the dataset has been successfully ingested under the index name `botsv1`
- Ensure the data status is active and searchable

###   Step 2: initial search for reconnaissance activity
Run the following SPL query in the **Search & Reporting** app:

index=botsv1 imreallynotbatman.com

Using this query, we look for the event logs in the index "botsv1" that contains the term imreallynotbatman.com.
Also, searching for the term imreallynotbatman.com in the index botsv1 will reveal important fields such as the 'host', 'source' and 'sourcetype' (left panel of the Splunk interface).
For example, you can see the log source 'stream:http' while clicking the 'sourcetype' field.

###  Step 3: Narrowing down the search for reconnaissance activity
Run the following SPL query in the **Search & Reporting** app. Or optionally, after Step 1 and after clicking the sourcetype, you can click the stream:http log source

index=botsv1 imreallynotbatman.com sourcetype=stream:http

As there are several source types as seen above, this query will help us to look for the term imreallynotbatman.comin in the stream:http log source 
(helping us to narrow down the search for the target scan attempt). 

###  Step 4: Identifying the IP address
As our main aim is to analyse for any IP address of attackers that attempt the reconnaissance,we have to look for another interesting field (in the left panel): the source_ip. 
While clicking on the source_ip field, it will have all the values it finds in the logs. In this case, two IP addresses are returned: 40.80.148.42 (93.4%) and 23.22.63.114 (6.6%).
Examining on each IP and looking at corresponding logs of each IP reveals that the former IP is related to the attacker attempting the reconnaissance activity. 

 
###  Step 5: Determining the CMS used by the webserver
Searching using the sourcetype 'suricata' can help us identify signature alerts information.
For example, using the following search query:

index=botsv1 imreallynotbatman.com src_ip="40.80.148.42" sourcetype=suricata 

Looking further at one of the returned event logs, it will show 'joomla' as the CMS used by the webserver. 


###  Step 6: Discovering the webserver's IP address
We can get IP address of the webserver directly from the logs — especially from the suricata logs involving HTTP traffic. This can be looking at the traffic of the suricate logs obtained using the 
query: index=botsv1 imreallynotbatman.com src_ip="40.80.148.42" sourcetype=suricata

Hence, we can see that the IP address of the webserver (imreallynotbatman.com) is 192.168.250.70.
 
 
###  Step 7: Determining web scanner or tool leveraged by the attacker
To identify the web scanner used by the attacker (40.80.148.42 IP address), we can use the http_user_agent field in Suricata HTTP logs. Apply the following query:

index=botsv1 imreallynotbatman.com src_ip="40.80.148.42" sourcetype=suricata | stats count by http.http_user_agent
Further digging at the http.http_user_agent output, we can see acunetix_wvs_security_test, which means the web scanner used by the attacker is Acunetix Web Vulnerability Scanner. 


###  Step 8: Analysing the suricata alerts and extract CVE value related to the attack attempt
To analyse the suricata alerts for traffic involving attacker IP 40.80.148.42 and extract any CVE ID associated with the detected attack, we can use the following query:
index=botsv1 imreallynotbatman.com src_ip="40.80.148.42" sourcetype=suricata | search signature="*CVE-*" | table _time, src_ip, dest_ip, signature

From the query results, the earliest suricata alert related to the attacker IP 40.80.148.42 shows signature!="ET WEB_SERVER Possible CVE-2014-6271 Attempt". 
Therefore, this confirms that the CVE value associated with the attack attempt is CVE-2014-6271. 
