---
title: "Week 6 Worklog"
date: 2026-05-25
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Week 6 Objectives:

* Monitor AWS systems with **Amazon CloudWatch**: Metrics, Logs, Alarms, and Dashboards.
* Understand AWS Support plans and manage support requests via **AWS Support**.
* Deploy a Hybrid DNS infrastructure integrating on-premises with AWS using **Route 53 Resolver** and **Microsoft AD**.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | - Learn Amazon CloudWatch & prepare environment <br> - **CloudWatch Metrics:** <br>&emsp; + View Metrics <br>&emsp; + Perform search operations <br>&emsp; + Perform math operations <br>&emsp; + Create dynamic labels | 25/05/2026 | 25/05/2026 | [What is Amazon CloudWatch?](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html) <br> [Using Amazon CloudWatch Metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/working_with_metrics.html) |
| 3 | - **CloudWatch Logs:** <br>&emsp; + View CloudWatch Logs <br>&emsp; + CloudWatch Logs Insights <br>&emsp; + CloudWatch Metric Filter <br> - Create CloudWatch Alarms <br> - Create CloudWatch Dashboards <br> - Clean up resources | 26/05/2026 | 26/05/2026 | [What is Amazon CloudWatch Logs?](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html) <br> [Creating CloudWatch Alarms](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html) <br> [CloudWatch Dashboards](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Dashboards.html) |
| 4 | - Learn about AWS Support Plans: Basic, Developer, Business, Enterprise On-Ramp, Enterprise <br> - Access AWS Support Console <br> - Learn about Support Request types (Account & billing, Technical) <br> - Change support plan | 27/05/2026 | 27/05/2026 | [AWS Support Plans](https://aws.amazon.com/premiumsupport/plans/) <br> [Getting started with AWS Support](https://docs.aws.amazon.com/awssupport/latest/user/getting-started.html) |
| 5 | - Create a Support Request (Support Case) <br> - Select Severity level <br> - Manage and track support request status | 28/05/2026 | 28/05/2026 | [Creating a support case](https://docs.aws.amazon.com/awssupport/latest/user/case-management.html) |
| 6 | - Read Hybrid DNS lab overview <br> - Generate Key Pair <br> - Initialize CloudFormation Template <br> - Configure Security Group <br> - Connect to RDGW (Remote Desktop Gateway) <br> - Deploy Microsoft Active Directory | 29/05/2026 | 29/05/2026 | [What is AWS Directory Service?](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/what_is.html) <br> [AWS CloudFormation User Guide](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html) |
| 7 - Sun | - Set up DNS with Route 53: <br>&emsp; + Create Route 53 Outbound Endpoint <br>&emsp; + Create Route 53 Resolver Rules <br>&emsp; + Create Route 53 Inbound Endpoints <br>&emsp; + Test results <br> - Clean up resources | 30/05/2026 | 31/05/2026 | [Forwarding outbound DNS queries](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resolver-forwarding-outbound-queries.html) <br> [Forwarding inbound DNS queries](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resolver-forwarding-inbound-queries.html) |

### Week 6 Achievements:

#### 1. Amazon CloudWatch Workshop Practice

##### **Environment Initialization (CloudFormation)**
* Successfully deployed the workshop CloudFormation template to set up the VPC structure, EC2 Instances, and relevant IAM policies.
* Configured and started the background log generator script `logger.py` on EC2 Instance A using Systems Manager (SSM) Run Command.
* Monitored and verified the stack creation success state on the AWS Console.

![Stack Created Successfully](/images/worklog/week-6/2.8.png)

##### **CloudWatch Metrics Actions**
* Checked resource performance utilization across instances, and configured dual Y-axes (Left and Right) to compare metrics with different measurement units.
* Defined visual warnings including a Horizontal Annotation (5% CPU mark) and a Vertical Annotation (representing "Job start" event at 02:40).

![CloudWatch Metrics and Annotations](/images/worklog/week-6/3.1.19.png)

##### **CloudWatch Logs & Logs Insights Management**
* Configured log retention settings for group `/ec2/linux/var/log/messages` from *Never expire* to *1 week (7 days)* to optimize storage costs.
* Ran Queries in Logs Insights to analyze real-time events and filter out rows containing `ERROR` levels.
* Created a Metric Filter `PythonAppErrors` to convert specific error log entries into a numeric metric.

![Metric Filter Created](/images/worklog/week-6/4.3.8.png)

##### **CloudWatch Alarm & SNS Notification Setup**
* Set up SNS Topic `Error_logs_reach_10` and subscribed email for automatic alerts.
* Created CloudWatch Alarm `PythonApplicationErrorAlarm` that triggers notifications when total error entries exceed 10 in a 1-minute period.

![CloudWatch Alarm Configured](/images/worklog/week-6/5.14.png)

##### **CloudWatch Dashboard Configuration**
* Built a custom dashboard `CloudWatch-Workshop` integrating both the Metric Filter error chart and the Alarm status monitoring widget, giving system operators an immediate, consolidated view of resource health.

![CloudWatch Dashboard](/images/worklog/week-6/6.5.png)

#### 2. Hybrid DNS – Route 53 Resolver Workshop

##### **2.1 Generate Key Pair**
* Created Key Pair `hybrid-DNS` (RSA, `.pem` format) used to decrypt the Administrator password for connecting to the RDGW instance.

##### **2.2 Initialize CloudFormation Template**
* Downloaded the template from the GitHub repository and deployed the `HybridDNS` CloudFormation stack with: Availability Zones `ap-southeast-1a` & `ap-southeast-1c`, Key Pair `hybrid-DNS`, Instance Type `t3.micro`.
* Stack completed successfully (`CREATE_COMPLETE`), provisioning a multi-AZ VPC, 2 public subnets, 2 private subnets, Internet Gateway, Route Table, Security Group, and a Windows Server 2022 RDGW EC2 instance.

![CloudFormation HybridDNS CREATE_COMPLETE](/images/worklog/week-6/2.2_cloudformation_complete.png)

##### **2.3 Configuring Security Group**
* Accessed the Security Group with description *"Enable RDP access from Internet"*.
* Deleted unnecessary inbound rules (Port 3391, Port 443).
* Restricted **RDP (3389)** and **ICMP** rules to only allow access from the lab machine's IP (`0.0.0.0/0` for lab scope) instead of the entire Internet.

##### **3. Connecting to RDGW**
* Downloaded the RDP file from EC2 Console and decrypted the Administrator password using `hybrid-DNS.pem`.
* Successfully connected to the Windows Server 2022 instance via Remote Desktop Protocol (RDP) at public IP `54.179.30.229`.

##### **4. Microsoft AD Deployment**
* Deployed **AWS Managed Microsoft AD** with the following configuration:
  * **Directory DNS name:** `onprem.example.com` (simulates the on-premises DNS system)
  * **NetBIOS name:** `onprem`
  * **Edition:** Standard
  * **VPC & Subnets:** Private subnet 1A (`ap-southeast-1a`) and Private subnet 2A (`ap-southeast-1b`)
* After ~20 minutes, status changed to **Active**, with 2 DNS IP addresses assigned: `10.0.25.100` and `10.0.44.109`.

![Microsoft AD Active](/images/worklog/week-6/4_microsoft_ad_active.png)

##### **5.1 Create Route 53 Outbound Endpoint**
* Created Outbound Endpoint `outbound-endpoint` (IPv4, Do53) attached to 2 private subnets (`ap-southeast-1a` and `ap-southeast-1b`), status **Operational**.
* This endpoint allows Route 53 Resolver to forward DNS queries from AWS to the on-premises DNS system (AWS Managed Microsoft AD).

![Outbound Endpoint Operational](/images/worklog/week-6/5.1_outbound_endpoint.png)

##### **5.2 Create Route 53 Resolver Rules**
* Created Resolver Rule `onprem-rule` of type **FORWARD** for domain `onprem.example.com`:
  * **Outbound Endpoint:** `outbound-endpoint`
  * **Target IPs:** `10.0.25.100:53` and `10.0.44.109:53` (2 AD DNS IPs)
* Associated the Rule with the `HybridDNS` VPC – all DNS queries for `onprem.example.com` within the VPC are forwarded to the AD DNS server.

![Resolver Rule onprem.example.com](/images/worklog/week-6/5.2_resolver_rule.png)

##### **5.3 Create Route 53 Inbound Endpoint**
* Created Inbound Endpoint `inbound-endpoint` (IPv4, Do53) attached to 2 private subnets, status **Operational**.
* This endpoint allows the on-premises DNS system (AWS Managed Microsoft AD) to send DNS queries back into Route 53 Resolver to resolve Private Hosted Zones in AWS.

![Inbound Endpoint Operational](/images/worklog/week-6/5.3_inbound_endpoint.png)

##### **5.4 Test Results**
* From the RDGW server, ran `nslookup onprem.example.com` and `Resolve-DnsName onprem.example.com` in PowerShell to verify that DNS resolution works correctly through the configured Hybrid DNS system.

##### **6. Clean Up Resources**
* Deleted resources in the correct order: **Inbound Endpoint** → **Disassociate & delete Resolver Rule** → **Outbound Endpoint** → **AWS Managed Microsoft AD** → **CloudFormation Stack HybridDNS** → **Key Pair**.
