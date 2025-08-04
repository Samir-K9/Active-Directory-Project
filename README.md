### Workflow Overview
![Image Alt](https://github.com/Samir-K9/Active-Directory-Project/blob/7356da9f2f730d95b6acf5fc67780fd1bb7adf5c/Screenshots/Screenshot%202025-08-01%20191510.png)

- **Telemetry Generation: An attacker machine successfully authenticates to the test machine which will then generate a telemetry to Splunk.**
- **Alert Generation: Splunk will then send a notification to Slack and also trigger a playbook on Shuffle.**
- **Alert Handling: Shuffle will send a notification email to the SOC analyst asking if they want to disable the user. If the SOC Analyst says NO, no further actions are taken but if the SOC Analyst says YES, it will instruct Shuffle to move onto the next step.**
- **Incident Management: Shuffle will instruct the domain controller to disable the domain user. Shuffle will then send a notification to Slack saying that the account has been disabled.**
- 
