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

##### **Infrastructure Setup (CloudFormation)**
* Created Key Pair `hybrid-DNS` and successfully deployed `HybridDNS` CloudFormation stack with a multi-AZ VPC, 4 subnets (2 public / 2 private), Security Group, and a Windows Server 2022 RDGW instance.

![CloudFormation HybridDNS CREATE_COMPLETE](/images/worklog/week-6/2.2_cloudformation_complete.png)

##### **Microsoft Active Directory Deployment**
* Successfully deployed AWS Managed Microsoft AD (`onprem.example.com`) simulating an on-premises DNS system. Status: **Active**.

![Microsoft AD Active](/images/worklog/week-6/4_microsoft_ad_active.png)

##### **Route 53 Resolver Configuration**
* Created **Outbound Endpoint** (`outbound-endpoint`) attached to 2 private subnets, status **Operational** – forwards DNS queries from AWS to the on-premises DNS server.
* Created **Resolver Rule** (`onprem-rule`) of type FORWARD for domain `onprem.example.com`, targeting the 2 AD DNS IPs and associated with the VPC.
* Created **Inbound Endpoint** (`inbound-endpoint`) status **Operational** – allows on-premises DNS to query internal AWS resources.

![Outbound Endpoint Operational](/images/worklog/week-6/5.1_outbound_endpoint.png)

![Resolver Rule onprem.example.com](/images/worklog/week-6/5.2_resolver_rule.png)

![Inbound Endpoint Operational](/images/worklog/week-6/5.3_inbound_endpoint.png)


