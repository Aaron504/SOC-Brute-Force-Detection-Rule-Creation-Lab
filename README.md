# SOC Brute Force Detection Rule Creation Lab
<p align="center">

  ![Screenshot 2024-10-13 135953](https://github.com/user-attachments/assets/539fe83b-599c-4a7d-9748-300fd7b6a74b)

</p>

In this lab, I manually created a custom detection rule in Azure Sentinel to identify brute force login attempts targeting Windows machines. The rule monitored SecurityEvent logs for EventID 4625 (failed logon attempts) and triggered when 10 or more failed attempts were detected within a 60-minute window. I tested the rule by generating failed logins and observing the results. After verifying the rule's functionality, I deleted it. This lab demonstrates my ability to create, test, and manage custom security rules for detecting brute force attacks in a SOC environment.




<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Microsoft Sentinel
- Kusto Query Language (KQL)

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Step 1 - Manually create the “TEST: Brute Force ATTEMPT - Windows” Rule


<h2>Deployment and Configuration Steps</h2>

- Within Azure Sentinel, browse to “Analytics” -> “Active Rules”
- Manually create the “TEST: Brute Force ATTEMPT - Windows” Rule
- SecurityEvent
| where EventID == 4625
| where TimeGenerated > ago(60m)
| summarize FailureCount = count() by AttackerIP = IpAddress, EventID, Activity, DestinationHostName = Computer
| where FailureCount >= 10
- Try to trigger it


![Screenshot 2024-10-13 130626](https://github.com/user-attachments/assets/83b0dd4e-d4a1-4356-970c-80e6ec969ed9)
![Screenshot 2024-10-13 131647](https://github.com/user-attachments/assets/9e9a929e-46e0-4547-9074-5675514a9611)
![Screenshot 2024-10-13 132238](https://github.com/user-attachments/assets/9b606547-00fb-49e8-8b24-c73dd48c57aa)

- Here I purposely did 10 failed log in attempts to trigger the rule we created in the previous step.
![Screenshot 2024-10-13 134227](https://github.com/user-attachments/assets/5228ce90-4520-4c2c-8dd8-5a4704ea3dc0)

- Here now you can see the brute force query we created in the previous has been triggered.
![Screenshot 2024-10-13 134527](https://github.com/user-attachments/assets/f9d56cd8-207b-4b2c-9ff0-bad6c0f34c52)

- Now you see the incident has been triggered in our SIEM
![Screenshot 2024-10-13 135225](https://github.com/user-attachments/assets/147870b8-4b53-4eb9-8e41-87e4225252c7)







