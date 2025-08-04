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
- **Download and install splunk on SAMIR-Splunk and make firewall rule changes to allow port access on 8000 to access Splunk web interface.**
![Image Alt](https://github.com/Samir-K9/Active-Directory-Project/blob/cdecf106246bc3b4467d32138a2b6a078f1444ba/Screenshots/Screenshot%202025-08-04%20160558.png)
- **Login to Splunk web interface and install Splunk Add-on for Microsoft Windows, create a new index called samir-ad and configure receiving on port 9997.**




