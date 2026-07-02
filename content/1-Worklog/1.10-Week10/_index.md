---
title: "Week 10 Worklog"
date: 2026-06-22
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

### Week 10 Objectives:

* Understand and configure **VPC Peering** to connect two separate VPC networks, configure routing, and set up **Network ACLs (NACLs)** for subnet-level stateless security control.
* Learn and deploy **AWS Transit Gateway (TGW)** to connect multiple VPCs in a Hub-and-Spoke model, simplifying large-scale network architecture.
* Implement serverless operations automation using **AWS Lambda** and **Amazon EventBridge** to automatically start/stop EC2 instances based on schedules and route execution logs/alerts to **Slack**.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | - Learn concepts of VPC Peering and Network Access Control Lists (NACL) <br> - **Lab 19 (Part 1):** Set up baseline network environment using CloudFormation, create Security Groups and EC2s | 22/06/2026 | 22/06/2026 | [VPC Peering Guide](https://docs.aws.amazon.com/vpc/latest/peering/what-is-vpc-peering.html) <br> [Network ACLs](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html) |
| 3 | - **Lab 19 (Part 2):** Configure custom Network ACLs, create VPC Peering connection, update route tables, and verify Cross-Peer DNS resolution | 23/06/2026 | 23/06/2026 | [VPC Peering DNS Support](https://docs.aws.amazon.com/vpc/latest/peering/modify-peering-connections.html) |
| 4 | - Learn concepts of AWS Transit Gateway (TGW) <br> - **Lab 20 (Part 1):** Set up 3 VPCs for demonstration, create Transit Gateway and configure VPC attachments | 24/06/2026 | 24/06/2026 | [AWS Transit Gateway Guide](https://docs.aws.amazon.com/vpc/latest/tgw/what-is-transit-gateway.html) |
| 5 | - **Lab 20 (Part 2):** Setup Transit Gateway Route Tables, add routes to VPC Route Tables, and verify cross-VPC network connectivity | 25/06/2026 | 25/06/2026 | [TGW Route Tables](https://docs.aws.amazon.com/vpc/latest/tgw/tgw-route-tables.html) |
| 6 | - Learn AWS Lambda Serverless compute and EventBridge scheduling services <br> - **Lab 22 (Part 1):** Set up Slack Incoming Webhooks, tag EC2 instances, and configure IAM Role for Lambda | 26/06/2026 | 26/06/2026 | [AWS Lambda Developer Guide](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html) <br> [Slack Webhooks](https://api.slack.com/messaging/webhooks) |
| 7 - Sun | - **Lab 22 (Part 2):** Implement Lambda Python code (boto3) to start/stop EC2s and send alerts to Slack, schedule jobs via EventBridge, and clean up resources | 27/06/2026 | 28/06/2026 | [Boto3 EC2 Client](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/ec2.html) <br> [EventBridge Scheduler](https://docs.aws.amazon.com/scheduler/latest/UserGuide/what-is-scheduler.html) |

### Week 10 Achievements:

* **VPC Peering & Security:** Successfully established a private network link between two isolated VPCs; applied Network ACLs to control inbound/outbound packets and gained deep knowledge of stateless NACL behavior versus stateful Security Groups.
* **Transit Gateway Deployment:** Implemented a centralized Hub-and-Spoke network interconnecting 3 VPCs using AWS Transit Gateway, eliminating the need for a complex full-mesh peering topology.
* **Serverless Automation:** Built a serverless management solution using AWS Lambda (Python `boto3`) that reads instance resource tags, stopping development hosts at night and starting them before business hours.
* **ChatOps Integration:** Integrated AWS Lambda alerts with Slack channel notifications using Incoming Webhooks for real-time operation status reporting.
* **Scheduled Operations:** Configured Amazon EventBridge Scheduler (cron triggers) to automatically invoke Lambda functions at specific times.

---

### Week 10 Practice Details:

#### Lab 1: VPC Peering & Network ACL (Lab 19)

##### 1. Introduction
In this lab, we connect two separate VPCs (**VPC-A** and **VPC-B**) using **VPC Peering**. Additionally, we configure a **Network Access Control List (NACL)**—a stateless subnet-level firewall—to evaluate how stateless rules differ from stateful security groups and learn how to manage inbound and outbound traffic.

##### 2. Prerequisites

###### 2.1 Initialize Infrastructure with CloudFormation
We use an AWS CloudFormation template to quickly provision the custom VPC.
* VPC 1 (My VPC): Default VPC `172.31.0.0/16`
* VPC 2 (HG VPC): Custom VPC `10.10.0.0/16`
* YAML CloudFormation template `VPCTemplate.yaml` defines VPC 2 resources:
```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: 'CloudFormation template to create a custom VPC 2 with CIDR 10.10.0.0/16'
Resources:
  HGVPCLab:
    Type: AWS::EC2::VPC
    Properties:
      CidrBlock: 10.10.0.0/16
      EnableDnsSupport: true
      EnableDnsHostnames: true
      Tags:
        - Key: Name
          Value: HG-VPC-Lab
```
* Verified CloudFormation stack execution output on the console:

![CloudFormation VPC](/images/worklog/week-10/1_cloudformation_create_vpc.png)

###### 2.2 Create Security Group
Create Security Groups for both VPCs:
* `SG-VPC-1`: Allows inbound SSH (Port 22) from your IP and ICMP (Ping) from VPC 2 CIDR (`10.10.0.0/16`).
* `SG-VPC-2`: Allows inbound SSH (Port 22) from your IP and ICMP (Ping) from VPC 1 CIDR (`172.31.0.0/16`).

###### 2.3 Launch EC2 Instance
* Deploy EC2 Host in the VPC 1 public subnet.
* Deploy EC2 Host in the VPC 2 public subnet.

##### 3. Configure VPC Peering
VPC Peering establishes a direct route for private IP traffic between two VPCs.
1. Go to the **VPC Console** -> **Peering connections** -> Click **Create peering connection**.
2. Configure settings:
   * **Name:** `MyVPC-to-HGVPC-Peering`
   * **VPC (Requester):** Select `VPC 1` (Default VPC)
   * **VPC (Accepter):** Select `VPC 2` (HG-VPC-Lab)
3. Click **Create peering connection**.
4. The connection status changes to `Pending acceptance`. Select it, click **Actions** -> **Accept request**.
5. Verified active VPC Peering status on the console:

![VPC Peering Active](/images/worklog/week-10/2_vpc_peering_active.png)

##### 4. Configure Route Tables
After accepting the peering connection, we must update the route tables so VPCs know how to route cross-VPC traffic.

* **VPC-A Route Table (Public Route Table A):**
  * Add Route: Destination `10.2.0.0/16` -> Target: `Peering Connection` (`pcx-xxxxxx`).
* **VPC-B Route Table (Public Route Table B):**
  * Add Route: Destination `10.1.0.0/16` -> Target: `Peering Connection` (`pcx-xxxxxx`).

{{% notice info "Connection Verification" %}}
SSH into **EC2-VPC-A** and ping the **Private IP** of **EC2-VPC-B**:
```bash
ping 10.2.1.X
```
If the ping is successful, the VPC Peering connection is active.
{{% /notice %}}

---

##### 5. Configure Network ACL (NACL) - Stateless Testing

{{% notice warning "Important NACL Rules" %}}
Unlike Security Groups which are **Stateful** (allowing inbound traffic implicitly allows outbound responses), Network ACLs are **Stateless**. You must explicitly define rules for both inbound and outbound traffic.
{{% /notice %}}

1. Create a custom NACL and associate it with VPC-A's Public Subnet.
2. Define the Inbound and Outbound rules to allow communication:
   * **Inbound Rules:**
     * Rule 100: Allow SSH (Port 22) from the internet (to connect to EC2).
     * Rule 110: Allow ICMP (Ping) from `172.31.0.0/16`.
     * Rule 120: Allow Ephemeral Ports (`1024-65535`) from anywhere.
   * **Outbound Rules:**
     * Rule 100: Allow ICMP (Ping) to `172.31.0.0/16`.
     * Rule 110: Allow Ephemeral Ports (`1024-65535`) to the internet.
3. **Test Ping Block:** Change the Inbound Rule 110 for ICMP from `Allow` to `Deny`. Ping from host inside VPC 1 to host inside VPC 2 to confirm that traffic is blocked.

##### 6. Enable Cross-Peer DNS Support
By default, EC2 instances cannot resolve the private DNS names of instances in peered VPCs. We must enable this setting:
1. Select **Peering Connections** -> Select `MyVPC-to-HGVPC-Peering`.
2. Click **Actions** -> Select **Edit VPC connection settings**.
3. Enable both:
   * **Requester DNS resolution:** `Allow accepter VPC to resolve DNS hostnames to private IPs`
   * **Accepter DNS resolution:** `Allow requester VPC to resolve DNS hostnames to private IPs`
4. Click **Save**. Test by resolving the private DNS hostname of the peered instance using `nslookup`.

##### 7. Resource Cleanup
Cleaned up services to prevent costs:
```bash
aws ec2 delete-vpc-peering-connection --vpc-peering-connection-id pcx-09f244ad36ffcd7ba
aws cloudformation delete-stack --stack-name "HG-VPC-Stack"
```

---

#### Lab 2: AWS Transit Gateway (Lab 20)

##### 1. Introduction
As the number of VPCs scales up, setting up full-mesh VPC Peering becomes complex. **AWS Transit Gateway (TGW)** acts as a central cloud router connecting VPCs in a Hub-and-Spoke topology, simplifying routing management and network scale-out.

In this lab, we build 3 VPCs: **VPC-A (10.1.0.0/16)**, **VPC-B (10.2.0.0/16)**, and **VPC-C (10.3.0.0/16)**. We configure them to route traffic chéo via a centralized Transit Gateway.

![Transit Gateway Hub and Spoke Architecture](/images/worklog/week-10/transit-gateway-diagram.png)

##### 2. Preparation
1. Create 3 independent VPCs with DNS Hostnames enabled:
   * **VPC-A:** CIDR `10.1.0.0/16` - Subnet: `10.1.1.0/24`
   * **VPC-B:** CIDR `10.2.0.0/16` - Subnet: `10.2.1.0/24`
   * **VPC-C:** CIDR `10.3.0.0/16` - Subnet: `10.3.1.0/24`
2. Create Security Groups permitting SSH and ICMP (Ping) on all 3 VPCs.
3. Deploy 1 EC2 instance in each VPC.

---

##### 3. Create Transit Gateway
1. Open the **VPC Console** -> **Transit gateways** -> Click **Create transit gateway**.
2. Enter values:
   * **Name:** `My-Transit-Gateway`
   * **Description:** Transit Gateway connecting VPC-A, B, and C
   * Keep other settings as default (Auto-accept Shared Attachments, Default Route Table Association, and Route Propagation).
3. Click **Create transit gateway** (creation takes 3-5 minutes, wait until state is `available`).

---

##### 4. Create Transit Gateway Attachments
Link each VPC to the Transit Gateway.

1. Select **Transit gateway attachments** -> Click **Create transit gateway attachment**.
2. Setup attachment for **VPC-A**:
   * **Transit gateway ID:** Select `My-Transit-Gateway`.
   * **Attachment type:** `VPC`
   * **Attachment name:** `Attach-VPC-A`
   * **VPC ID:** Select `VPC-A`
   * **Subnet:** Select the subnet of VPC-A.
3. Click **Create attachment**.
4. Repeat the steps for **VPC-B** (`Attach-VPC-B`) and **VPC-C** (`Attach-VPC-C`). Wait until all attachments show `Associated`.

---

##### 5. Configure Routing on VPC Route Tables
To forward traffic from the VPCs to the Transit Gateway, we need to add routes in each VPC's Route Table.

* **VPC-A Route Table:**
  * Add Route: Destination `10.2.0.0/16` -> Target: `Transit Gateway` (`tgw-xxxx`)
  * Add Route: Destination `10.3.0.0/16` -> Target: `Transit Gateway` (`tgw-xxxx`)
* **VPC-B Route Table:**
  * Add Route: Destination `10.1.0.0/16` -> Target: `Transit Gateway` (`tgw-xxxx`)
  * Add Route: Destination `10.3.0.0/16` -> Target: `Transit Gateway` (`tgw-xxxx`)
* **VPC-C Route Table:**
  * Add Route: Destination `10.1.0.0/16` -> Target: `Transit Gateway` (`tgw-xxxx`)
  * Add Route: Destination `10.2.0.0/16` -> Target: `Transit Gateway` (`tgw-xxxx`)

---

##### 6. Verification
1. SSH into **EC2-VPC-A** (`10.1.1.X`).
2. Ping **EC2-VPC-B** (`10.2.1.Y`) and **EC2-VPC-C** (`10.3.1.Z`):
   ```bash
   ping 10.2.1.Y
   ping 10.3.1.Z
   ```
3. If responses are received, the three VPC networks are communicating successfully through the Transit Gateway.

---

##### 7. Resource Cleanup
* Terminate the EC2 Instances.
* Select and delete all 3 attachments (`Attach-VPC-A`, `Attach-VPC-B`, `Attach-VPC-C`) under **Transit gateway attachments**.
* Delete `My-Transit-Gateway` under **Transit gateways**.
* Delete VPC-A, VPC-B, and VPC-C.

---

#### Lab 3: EC2 Automation with AWS Lambda & Slack Notification (Lab 22)

##### 1. Introduction
Cost Optimization is a key pillar of the AWS Well-Architected Framework. In practice, development or testing servers do not need to run 24/7. This lab builds a Serverless automation workflow: **AWS Lambda** runs daily, filtering instances by tag, automatically stopping them at 8:00 PM (end of day) and starting them at 8:00 AM (start of day), sending execution status alerts directly to **Slack**.

![Lambda EC2 Automation Flow](/images/worklog/week-10/lambda-ec2-diagram.png)

##### 2. Preparation

###### 2.1 Set Up EC2 Instance
* Deploy an EC2 Instance using Amazon Linux 2023.
* Add tags to the instance:
  * **Key:** `Auto-Stop-Start`
  * **Value:** `true`

###### 2.2 Create Slack Incoming Webhook
To send notifications from AWS Lambda to Slack:
1. Go to the [Slack API Console](https://api.slack.com/apps) -> Click **Create New App** -> Choose **From scratch**.
2. Name the app `AWS-Ops-Bot` and choose your Workspace.
3. Select **Incoming Webhooks**, toggle it to **On**.
4. Click **Add New Webhook to Workspace**, select the target channel (e.g., `#aws-alerts`), and authorize it.
5. Copy the Webhook URL (format: `https://hooks.slack.com/services/TXXXXX/BXXXXX/XXXXXXXXXX`).

---

##### 3. Create IAM Role for Lambda Function
Lambda needs IAM permissions to describe, start, and stop EC2 instances, and write Execution logs to CloudWatch Logs.

1. Go to **IAM Console** -> **Roles** -> Click **Create role**.
2. Select **Lambda** as the trusted service.
3. Attach a custom inline policy with the following JSON:
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ec2:DescribeInstances",
                "ec2:StartInstances",
                "ec2:StopInstances"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "logs:CreateLogGroup",
                "logs:CreateLogStream",
                "logs:PutLogEvents"
            ],
            "Resource": "arn:aws:logs:*:*:*"
        }
    ]
}
```
4. Name the IAM Role `Lambda-EC2-Slack-Role`.

---

##### 4. Deploy AWS Lambda Functions
We implement Python code using the `boto3` SDK to control resources.

###### 4.1 Create the Stop Function (`EC2-Auto-Stop`)
1. Open the **AWS Lambda Console** -> Click **Create function**.
2. Configuration details:
   * **Function name:** `EC2-Auto-Stop`
   * **Runtime:** `Python 3.12`
   * **Execution role:** Select *Use an existing role*, choose `Lambda-EC2-Slack-Role`.
3. In the Code source editor, enter the following code:

```python
import json
import urllib.request
import boto3

ec2 = boto3.client('ec2', region_name='ap-southeast-1')
SLACK_WEBHOOK_URL = "https://hooks.slack.com/services/TXXXXX/BXXXXX/XXXXXXXXXX" # Replace with your webhook

def lambda_handler(event, context):
    # Filter running instances with tag Auto-Stop-Start = true
    filters = [
        {'Name': 'tag:Auto-Stop-Start', 'Values': ['true']},
        {'Name': 'instance-state-name', 'Values': ['running']}
    ]
    
    response = ec2.describe_instances(Filters=filters)
    instance_ids = []
    
    for reservation in response['Reservations']:
        for instance in reservation['Instances']:
            instance_ids.append(instance['InstanceId'])
            
    if len(instance_ids) > 0:
        ec2.stop_instances(InstanceIds=instance_ids)
        message = f"🛑 *AWS Lambda Alert:* Successfully stopped EC2 Instances: {', '.join(instance_ids)}"
    else:
        message = "ℹ️ *AWS Lambda Info:* No running EC2 Instances found with tag `Auto-Stop-Start: true` to stop."
        
    # Send message to Slack
    slack_message = {"text": message}
    req = urllib.request.Request(
        SLACK_WEBHOOK_URL,
        data=json.dumps(slack_message).encode('utf-8'),
        headers={'Content-Type': 'application/json'}
    )
    urllib.request.urlopen(req)
    
    return {
        'statusCode': 200,
        'body': json.dumps(message)
    }
```

4. Click **Deploy**.

###### 4.2 Create the Start Function (`EC2-Auto-Start`)
1. Create a new function named `EC2-Auto-Start` with the same configuration.
2. In the Code source editor, enter:

```python
import json
import urllib.request
import boto3

ec2 = boto3.client('ec2', region_name='ap-southeast-1')
SLACK_WEBHOOK_URL = "https://hooks.slack.com/services/TXXXXX/BXXXXX/XXXXXXXXXX" # Replace with your webhook

def lambda_handler(event, context):
    # Filter stopped instances with tag Auto-Stop-Start = true
    filters = [
        {'Name': 'tag:Auto-Stop-Start', 'Values': ['true']},
        {'Name': 'instance-state-name', 'Values': ['stopped']}
    ]
    
    response = ec2.describe_instances(Filters=filters)
    instance_ids = []
    
    for reservation in response['Reservations']:
        for instance in reservation['Instances']:
            instance_ids.append(instance['InstanceId'])
            
    if len(instance_ids) > 0:
        ec2.start_instances(InstanceIds=instance_ids)
        message = f"🚀 *AWS Lambda Alert:* Successfully started EC2 Instances: {', '.join(instance_ids)}"
    else:
        message = "ℹ️ *AWS Lambda Info:* No stopped EC2 Instances found with tag `Auto-Stop-Start: true` to start."
        
    # Send message to Slack
    slack_message = {"text": message}
    req = urllib.request.Request(
        SLACK_WEBHOOK_URL,
        data=json.dumps(slack_message).encode('utf-8'),
        headers={'Content-Type': 'application/json'}
    )
    urllib.request.urlopen(req)
    
    return {
        'statusCode': 200,
        'body': json.dumps(message)
    }
```

3. Click **Deploy**.

---

##### 5. Configure Triggers via Amazon EventBridge Scheduler
Set up recurring schedules to execute the Lambda functions.

1. Go to **Amazon EventBridge** -> **Schedulers** -> Click **Create schedule**.
2. **Auto-Stop Schedule Configuration:**
   * **Schedule name:** `Daily-EC2-Stop`
   * **Occurrence:** `Recurring schedule`
   * **Schedule type:** `Cron-based schedule`
   * **Expression:** `0 20 ? * MON-FRI *` (Runs at 8:00 PM UTC/Local, Monday through Friday).
   * **Target:** Select `AWS Lambda` -> Choose `EC2-Auto-Stop`.
   * Click **Create schedule**.
3. **Auto-Start Schedule Configuration:**
   * **Schedule name:** `Daily-EC2-Start`
   * **Occurrence:** `Recurring schedule`
   * **Schedule type:** `Cron-based schedule`
   * **Expression:** `0 8 ? * MON-FRI *` (Runs at 8:00 AM UTC/Local, Monday through Friday).
   * **Target:** Select `AWS Lambda` -> Choose `EC2-Auto-Start`.
   * Click **Create schedule**.

---

##### 6. Check Result
1. Run a manual test by clicking **Test** in the `EC2-Auto-Stop` function console.
2. Check the EC2 Instances page to see if your instance transitions from `running` to `stopping`/`stopped`.
3. Check your **Slack** channel to confirm the alert notification is posted.

![Ops Bot Slack Notification](/images/worklog/week-10/slack-notification.png)

---

##### 7. Clean up resources
* Delete both EventBridge Schedules.
* Delete both Lambda Functions (`EC2-Auto-Stop`, `EC2-Auto-Start`).
* Delete the IAM Role `Lambda-EC2-Slack-Role`.
* Terminate the target EC2 Instance.
