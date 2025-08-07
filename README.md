# Active-Directory-Project

## Objective
The primary objective of this project was to create a fully functional Active Directory environment in the VULTR cloud platform, implement Splunk for security monitoring and alerting to detect unauthorized logings and integrate Shuffle as a SOAR platform to automate incident response workflows. 

## Tools and Platforms
- **VULTR cloud platform**
- **Windows Server 2022**
- **Ubuntu 22.04**
- **Splunk Enterprise**
- **Shuffle**
- **Slack**

## Workflow Overview
![Image Alt](https://github.com/Samir-K9/Active-Directory-Project/blob/7356da9f2f730d95b6acf5fc67780fd1bb7adf5c/Screenshots/Screenshot%202025-08-01%20191510.png)

- **Telemetry Generation: An attacker machine successfully authenticates to the test machine which will then generate a telemetry to Splunk.**
- **Alert Generation: Splunk will then send a notification to Slack and also trigger a playbook on Shuffle.**
- **Alert Handling: Shuffle will send a notification email to the SOC analyst asking if they want to disable the user. If the SOC Analyst says NO, no further actions are taken but if the SOC Analyst says YES, it will instruct Shuffle to move onto the next step.**
- **Incident Management: Shuffle will instruct the domain controller to disable the domain user. Shuffle will then send a notification to Slack saying that the account has been disabled.**

## Steps
### Step 1: Deploy VMs on the VULTR cloud platform.
- **SAMIR-ADDC01: Windows Standard 2022 server with 2 vCPUs, 4GB RAM, and 80GB disk space.**
- **Cloud instance: Windows Standard 2022 server with 1 vCPU, 2GB RAM, and 55GB disk space.**
- **SAMIR-SPLUNK: Ubuntu 22.04 server with 4 vCPUs, 8GB RAM, and 160GB disk space.**
![Image Alt](https://github.com/Samir-K9/Active-Directory-Project/blob/c9d4ea312d73e33c5bddcc8ea441df0e99a21c44/Screenshots/Screenshot%202025-08-03%20131251.png)

### Step 2: Configure firewall settings.
- **Create a firewall group from under Network option in VULTR and restrict access to only SSH (port 22) and MS RDP (3389) from your public address.**
![Image Alt](https://github.com/Samir-K9/Active-Directory-Project/blob/cbafe7f0777afafbfb6d06b84c8e5653ab55c2f9/Screenshots/Screenshot%202025-08-03%20132413.png)
- **Add the created firewall rules to all the VMs.**

### Step 3: Enable VPC Network.
- **Enable VPC for all three VMs, assigning them private IP addresses within the 10.22.96.x range.**

### Step 4: Active Directory Configuration
- **Install Active Directory Domain Services on SAMIR_ADDC01 with domain name samir.local.**
- **Create a new user: Jenny Smith with login name JSmith.**
![Image Alt](https://github.com/Samir-K9/Active-Directory-Project/blob/cbafe7f0777afafbfb6d06b84c8e5653ab55c2f9/Screenshots/Screenshot%202025-08-04%20125426.png)

### Step 5: Join the domain from Cloud Instance.
- **In Cloud Instance, set the DNS server to the IP address of SAMIR-ADDC01 and join the domain samir.local.**
- **Login using the credentials of Jenny Smith to make sure everything is working properly.**

### Step 6: Install Splunk on SAMIR-Splunk
- **Download and install splunk on SAMIR-Splunk. Make firewall rule changes to allow port access on 8000 to access Splunk web interface.**

![Image Alt](https://github.com/Samir-K9/Active-Directory-Project/blob/cdecf106246bc3b4467d32138a2b6a078f1444ba/Screenshots/Screenshot%202025-08-04%20160558.png)

- **Login to Splunk web interface and install Splunk Add-on for Microsoft Windows, create a new index called samir-ad and configure receiving on port 9997.**

### Step 7: Install Splunk Universal Forwarder.
- **Install Splunk Universal Forwarder on Cloud Instance and SAMIR-ADDC01 and copy the inputs.conf file from C:\Program Files\SplunkUniversalForwarder\etc\system\default to C:\Program Files\SplunkUniversalForwarder\etc\system\local. Add the following in the inputs.conf file by opening the file using Notepad as administrator:**
 ```
[WinEventLog://Security]
index = samir-ad
disabled = false
```
- **Add the firewall rule in SAMIR-Splunk to view the events in Splunk. Use the command:**
 ```
ufc allow 9997
 ```
### Step 8: Create alert for Unauthorized Successful RDP logins.
- **Create a query to identify unauthorized successful RDP login. The query used:**
 ```
index="samir-ad" EventCode=4624 (Logon_Type=7 OR Logon_Type=10) Source_Network_Address=* Source_Network_Address!=*-* Source_Network_Address!=40.* |stats count by _time,ComputerName,Source_Network_Address,user,Logon_Type
 ```
-**Save and configure the scheduled alert to trigger if unauthorized RDP logins are detected.**

### Step 9: Shuffle Setup
- **Create an account on Shuffle and create a new workflow Samir-AD-Project.**
- **Create a Webhook which will show Splunk alerts in the workflow.**

### Step 10: Slack and Email Integration
- **Create a slack workspace and create a new channel called alerts where the alerts will be sent in from Shuffle to notify the SOC Analyst.**
- **In the workflow, add a "User Input" node to send an email notification to SOC analyst's email address.**

### Step 11: Integrate Active Directory to disable user.
- **Integrate Active Directory in Shuffle such that if the SOC Anlyst wants to disable the user it can be automated.**
- **The workflow looks like this:**
  ![Image Alt](https://github.com/Samir-K9/Active-Directory-Project/blob/73fce775107ac40a016edc3420bd42d92268cff5/Screenshots/Screenshot%202025-08-06%20174734.png)

### Conclusion
This project has significantly helped me to enhance my practical skills in deploying, securing and automating enterprise-level architecture. Hands-on experience with network design, Active Directory configuration, threat detection using Splunk, and incident response automation with Shuffle have helped me develop a well rounded skill in security operations and take on real-world challenges as a SOC Analyst.

### References
- https://www.youtube.com/@MyDFIR








