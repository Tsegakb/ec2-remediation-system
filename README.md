The EC2 Remediation System is a ServiceNow-based workflow designed to detect, notify, and remediate failing AWS EC2 instances that impact Netflix's streaming infrastructure. The system is built to support  reduce incident response time, and integrate with DevOps workflows via Slack, AI Search.

This application addresses the issue of delayed EC2 instance failure detection by automating:
Incident creation for failed instances
AI-powered knowledge retrieval
Slack notifications with actionable guidance
Manual remediation via one-click UI Action
Comprehensive logging for auditing and postmortem analysis
 Implementation Steps
1. Scoped Application Setup

Name: EC2 Monitoring and Remediation
Required for AWS Integration Server compatibility.
2. Custom Tables
3. AWS Integration Configuration
Connection & Credential Alias: AWS Integration Server C C Alias
Connection:
Host: codon-staging.emaginelc.com
Base path: /api/v1/queue/start
Credentials:
Type: Basic Auth
Username: admin
Password: (ServiceNow instance password)
![Connection](https://github.com/user-attachments/assets/778ab5e7-3e7e-4014-be4d-33ae8f778817)

4. UI Action & Script Include
![Action](https://github.com/user-attachments/assets/d44acef1-c637-4c6d-96b8-005448280af6)
UI Action Name: Trigger EC2 Remediation
  Table: EC2 Instance
   Type: Form Button
Client Script: trigger_EC2_Remediation.js
Script Include: EC2RemediationHelper
API Name: x_snc_ec2_monito_0.EC2RemediationHelper
5. Flow Designer Workflow
Trigger Condition: EC2 Instance status changes to OFF
Steps:
     AI Search Custom Action: Retrieves related knowledge articles
     Slack Webhook Notification: Sends article summary and remediation prompt
     Incident Creation: Logged in Incident table
     Use Force Save to ensure full inclusion in update sets


6. AI Search Integration
AI Search Custom Action parameters:
Search Term: "EC2 instance OFF" or related phrases
Enable Logging: true
Search App: Based on instance's AI Search
<img width="1160" height="1491" alt="Diagram" src="https://github.com/user-attachments/assets/f23ce384-fff3-48e9-90e5-3e418a91cde2" />

Optimization: I would Improve system efficiency, reduce latency, and increase remediation reliability during EC2 instance 
failures.

 Step-by-Step Usage Guide
1. Monitor Slack Alerts

When an EC2 instance status changes to OFF, you’ll receive a Slack notification in the assigned DevOps channel.
2. Navigate to ServiceNow

Go to: ServiceNow → EC2 Monitoring and Remediation → EC2 Instances
Use the filter to locate instances with status = OFF

3. Open the Failed Instance Record
4. Click "Trigger EC2 Remediation"
Button is available on the form view of EC2 Instance table
You’ll receive visual confirmation or an error if remediation fails
5. Check the Remediation Log
Navigate to: EC2 Monitoring and Remediation → Remediation Logs
Search for the instance ID or filter by the most recent attempt
6. Follow-Up
If remediation fails, escalate(NEED OPTIMAZATION) 
If successful, monitor for instance status to return to ON

Close the related incident ticket if resolved
