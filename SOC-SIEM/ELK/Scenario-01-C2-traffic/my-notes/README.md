 # SOC monitoring scenario: Investigate a potential C2 communication alert using Elastic stack (ELK)

 #$$$ Scenario $$$$$$
During routine SOC monitoring, an IDS alert indicated potential Command and Control (C2) activity originating 
from a user named Browne in the HR department. The alert was triggered after a suspicious file containing 
a malicious pattern (THM:{_________}) was accessed. Due to resource constraints, only HTTP connection logs 
from the past week were available and ingested into the connection_logs index in Kibana for further investigation.


# Objective
The purpose of this investigation is to analyse 'HTTP' connection logs from the Kibana's interface 
of the connection_logs index to find evidence of potential C2 (Command & Control) communication 
involving user Browne. More specifically, the analysis focuses to:
- Identify the total number of events returned in March 2022.
- Determine the source IP address related to a suspected user in the logs?
- Find the name of the legitimate Windows binary used to download a suspicious file
- Determine the filesharing platform utilized by the malware authors as a C2 channel.
- Extract the complete URL of the C2 endpoint used by the infected machine.
- Retrieve the name of the accessed file from the filesharing website.
- Obtain the secret flag embeded in the accessed file with the format THM{________}.


# $$$$$$      Steps 

### Step 1
- Steps taken to identify the total number of events returned in March 2022:
 * First, in the Kibana of the ELK interface I applied a time filter that can allow me to determine the log filter based on the specified time, that is for the month of March 2022. 
   (March 1, 2022 @00:00:00.00 -> March 31, 2022 @23:30:00.00)
 * Next, hitting the 'Update' to apply for the time filter returned a total number of 1482 hits for the month March 2022.

### Step 2
- Steps taken to determine the source IP address related to a suspected user in the logs
  * First, I created a table by clicking on the 'Time' column on any line/row of the 'Time' and 'Document' table   
  * I used the source_ip to toggle column in the table, creating a table with 
   'Time' and 'source_ip' columns
  * Now, under the 'Selected fields' click the source_ip to see the source-ip visualisation. 
  * The visualisation yields only two source IP addresses: 192.166.65.52 and 192.166.65.54. 
  * And, further investigating at both IPs in the logs, it's more likely that 192.166.65.54 is the source IP related to the suspected user 
  

### Step 3
- Steps taken to find the name of the legitimate Windows binary used to download a suspicious file
  * Following Step 2 and using the search query of 'source_ip: 192.166.65.54', check the expanded document across one of the rows related to the source IP 192.166.65.54
  * Keep note of the user_agent 'bitsadmin' and further exploring and researching bitsadmin, bitsadmin can be used to download a suspocious file
  * The MITRE ATT&CK resource (refer to the Procedure ID of S0190) at https://attack.mitre.org/techniques/T1197/ 
   specifically mentions that BITSAdmin can be used to create BITS Jobs to launch a malicious process. 


### Step 4
- Steps taken to determine the filesharing platform utilized by the malware authors as a C2 channel
  * Similar to Step 3 above, check the 'host' field of the expanded document under any row of the filtered table related to the source IP 192.166.65.54
  * Here, the host pastebin.com indicates as the filesharing platform utilized by the malware authors

### Step 5
- Steps taken to extract the complete URL of the C2 endpoint used by the infected machine  
  * Similar to Step 4 above, check the 'uri' filed. So, we got uri of /yTg0Ah6a and we have the host of pastebin.com from Step 4.
  * So, the complete URL of the C2 endpoint would be pastebin.com/yTg0Ah6a


### Step 5
- Steps taken to retrieve the name of the accessed file from the filesharing site
  * Use the complete URL of pastebin.com/yTg0Ah6a and open in browser. 
  * You'd see secret.txt is the name of the file


### Step 6
- Steps taken to obtain the secret flag embeded in the accessed file of secret.txt with the format THM{________} 
  * You'd see the flag THM{SECRET__CODE} under secret.txt within the URL 
