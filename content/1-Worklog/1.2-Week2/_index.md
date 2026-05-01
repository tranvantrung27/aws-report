---
title: "Week 2 Worklog"
date: 2026-04-27
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

### Week 2 Objectives:

* Understand network architecture in AWS (VPC).
* Master how to configure network components and security within a VPC.
* Practice deploying a complete VPC.

### Tasks to be carried out this week:
| Day     | Task                                                                                                                                                                                          | Start Date | Completion Date | Reference Material                                                                                            |
| ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ------------------------------------------------------------------------------------------------------------- |
| 2       | - Learn VPC basics <br>&emsp; + What is VPC <br>&emsp; + CIDR Block <br>&emsp; + Distinguish Public / Private Subnets                                                                         | 27/04/2026 | 27/04/2026      | [VPC Basics](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html)                        |
| 3       | - Learn about Subnets & Route Tables <br>&emsp; + How to divide subnets <br>&emsp; + Configure Route Tables                                                                                   | 28/04/2026 | 28/04/2026      | [Subnets & Route Table](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html)                    |
| 4       | - Learn about Internet Gateway & NAT Gateway <br>&emsp; + Internet connection for Public Subnets <br>&emsp; + NAT for Private Subnets                                                         | 29/04/2026 | 29/04/2026      | [NAT Gateways](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html)                         |
| 5       | - Learn about security in VPC <br>&emsp; + Security Groups <br>&emsp; + Network ACLs <br>&emsp; + Inbound / Outbound rules                                                                    | 30/04/2026 | 30/04/2026      | [VPC Security Groups](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html)               |
| 6       | - **Practice:** <br>&emsp; + Create VPC <br>&emsp; + Create Public & Private Subnets <br>&emsp; + Configure Route Tables                                                                      | 01/05/2026 | 01/05/2026      | [FCJ Lab - Create VPC](https://cloudjourney.awsstudygroup.com/2-prerequisites/2.1-createvpc/)                 |
| 7 - Sun | - **Advanced practice:** <br>&emsp; + Attach Internet Gateway <br>&emsp; + Create NAT Gateway <br>&emsp; + Deploy EC2 into Public / Private Subnets <br>&emsp; + Verify Internet connectivity | 02/05/2026 | 03/05/2026      | [FCJ Lab - Public/Private](https://cloudjourney.awsstudygroup.com/2-prerequisites/2.2-publicprivateinstance/) |

### Week 2 Achievements:

**Theory:**
* Clearly understood the architecture and operation of Amazon VPC, and how to manage IP address ranges via CIDR.
* Learned how to partition Subnets (Public/Private) and configure routing via Route Tables.
* Mastered Security Group and NACL layers to protect resources within the VPC.

**Practice:**
* **Amazon VPC:** Successfully deployed a custom VPC network model with all components (IGW, NAT Gateway).
* **Network Connectivity:** Successfully configured secure Internet access for resources within the Private Subnet.
* **Testing & Deployment:** Deployed EC2 Instances and performed real-world connectivity tests, ensuring the network system operates as designed.

---

### Detailed Practice for Week 2:

#### 1. Launching Amazon VPC
- **Description:** Created a custom VPC named **ASG** to establish an isolated network environment.
- **Operations:** 
    - Selected **VPC only**.
    - Name tag: **ASG**.
    - IPv4 CIDR: **10.10.0.0/16**.
- **Image:**
  * Interface confirming successful VPC creation:
![VPC Successfully Created](/images/worklog/week-2/1.png)
  * Details of the ASG VPC configuration:
![VPC Configuration Details](/images/worklog/week-2/2.png)

#### 2. Configuring Subnets
- **Description:** Partitioned the VPC into 4 zones, including 2 Public Subnets and 2 Private Subnets for high availability.
- **Operations:** 
    - Created subnets: Public Subnet 1, Public Subnet 2, Private Subnet 1, and Private Subnet 2.
    - Assigned IP ranges for each subnet (from `10.10.1.0/24` to `10.10.4.0/24`).
- **Image:**
  * List of successfully initialized Subnets (Public/Private):
![List of Created Subnets](/images/worklog/week-2/3.png)


#### 3. Setting Up Internet Gateway (IGW)
- **Description:** An Internet Gateway is a VPC component that allows communication between your VPC and the internet, providing a connection point and supporting two-way communication for resources.
- **Operations:** 
    - Accessed the VPC Console and selected **Internet Gateways**.
    - In the **Name tag** field, entered **Internet Gateway**.
    - Selected **Actions** -> **Attach to VPC** to attach the IGW to the **ASG** VPC.
- **Image:**
  * Screen confirming successful Internet Gateway creation:
![Internet Gateway Successfully Created](/images/worklog/week-2/4.png)
  * Internet Gateway status attached to the ASG VPC:
![Internet Gateway Successfully Attached to VPC](/images/worklog/week-2/5.png)

#### 4. Configuring Route Tables
- **Description:** A Route Table contains a set of rules (routes) used to determine where network traffic is directed within the VPC.
- **Operations:** 
    - Created a Route Table named **Route table-Public** and assigned it to the **ASG** VPC.
    - Added a new route: Destination **0.0.0.0/0** (Internet), Target selected the created **Internet Gateway**.
    - Performed **Subnet associations** to link the Public Subnets to this Route Table.
- **Image:**
  * Screen confirming successful Route Table creation:
![Route Table Successfully Created](/images/worklog/week-2/6.png)
  * Details of the route configuration to Internet (0.0.0.0/0) via IGW:
![Adding New Route via Internet Gateway](/images/worklog/week-2/7.png)
  * Confirmation of successful Subnet associations to the Route Table:
![Subnet Associations Confirmed Successfully](/images/worklog/week-2/8.png)


#### 5. Configuring Security Groups (SG)
- **Description:** Security Groups act as a virtual firewall for your instances to control inbound and outbound traffic at the instance level.
- **Operations:** 
    - **Public Subnet SG:** Created a Security Group named **Public subnet - SG** to allow SSH and Ping access for public servers.
    - **Private Subnet SG:** Created a Security Group named **Private subnet - SG** to manage access for private resources.
    - **VPC Endpoints SG:** Created a Security Group named **VPC-Endpoints-SG**, configured Inbound rules to allow **HTTPS (port 443)** traffic from the VPC network range (**10.10.0.0/16**).
    - Updated Outbound rules for the Private SG to allow HTTPS traffic to VPC Endpoints.
- **Image:**
  * Screen confirming successful Public Subnet SG creation:
![Public subnet - SG Created Successfully](/images/worklog/week-2/9.png)
  * Screen confirming successful Private Subnet SG creation:
![Private subnet - SG Created Successfully](/images/worklog/week-2/10.png)
  * Successful configuration of the Security Group for VPC Endpoints:
![VPC Endpoints SG Configured Successfully](/images/worklog/week-2/11.png)

#### 6. Enabling VPC Flow Logs
- **Description:** VPC Flow Logs capture information about IP traffic going to and from network interfaces in your VPC, assisting in security monitoring and network troubleshooting.
- **Operations:** 
    - Selected the **ASG** VPC, navigated to the **Flow logs** tab, and clicked **Create flow log**.
    - Configured Filter: **All**, Aggregation interval: **1 minute**, Destination: **CloudWatch Logs**.
    - Destination log group: **/aws/vpc/flowlogs**.
    - Tag: Name - **ASG-VPC-FlowLogs**.
- **Image:**
  * Screen confirming successful VPC Flow Logs activation:
![VPC Flow Logs Enabled Successfully](/images/worklog/week-2/12.png)

#### 7. Testing and Deploying EC2
- **Description:** Deployed a comprehensive EC2 infrastructure (Public/Private) to test routing, connectivity, and security following AWS best practices for high availability and reliability.
- **Operations:** 
    - **Create EC2 Public:** Named **EC2 Public**, Instance ID: **i-0fca534a121c6972b**. Assigned to **Public Subnet 1**, enabled **Auto-assign public IP**, used Key pair **aws-keypair** and the existing **Public subnet - SG**.
    - **Create EC2 Private:** Named **EC2 Private**, assigned to the **Private Subnet**, and used the **Private subnet - SG**. Recorded the Private IP (e.g., `10.10.4.116`).
    - **SSH Connection via Visual Studio Code:**
        1. Installed the **Remote - SSH** extension in VS Code.
        2. Configured the SSH `config` file with: **HostName** (Public IP), **User** (ec2-user), and **IdentityFile** (Absolute path to the `aws-keypair.pem` file).
        3. Selected the configured host and established the connection, confirming the server's **Fingerprint** to complete access.
    - **Status Check:** Monitored the instances until the **Status check** displayed **2/2 checks passed**, ensuring the instances were running correctly.
- **Image:**
  * Screen confirming successful EC2 Public initialization:
![EC2 Public Initialized Successfully](/images/worklog/week-2/13.png)
  * EC2 instance state showing 2/2 status checks passed:
![EC2 Status Checks Passed Successfully](/images/worklog/week-2/14.png)
  * Screen confirming successful EC2 Private initialization and recording private IP details:
![EC2 Private Initialized Successfully](/images/worklog/week-2/15.png)
  * Terminal interface confirming successful SSH connection to the EC2 Public instance:
![SSH Connection Successful](/images/worklog/week-2/16.png)

#### 8. Setting Up NAT Gateway
- **Description:** NAT Gateway allows instances in a private subnet to connect to the internet, ensuring a one-way connection from the inside out to enhance system security.
- **Operations:** 
    - **Create Elastic IP Address:** Accessed the EC2 Dashboard, selected **Elastic IPs**, and clicked **Allocate Elastic IP address** to assign a static IP.
    - **Deploy NAT Gateway:** In the VPC Dashboard, selected **NAT Gateways**, and clicked **Create NAT gateway**. Configured Name: **NAT gateway**, Subnet: **Public subnet 2**, Connectivity type: **Public**, and assigned the created Elastic IP.
    - **Configure Route Table for Private Subnet:** 
        - Created a new Route table named **Route table - Private**, assigned to **VPC ASG**.
        - **Subnet Associations:** Linked both Private Subnets to this Route Table.
        - **Routes:** Added a new route with Destination **0.0.0.0/0** and Target pointing to the initialized **NAT Gateway**.
- **Image:**
  * Confirmation of successful Elastic IP creation:
![Elastic IP Successfully Allocated](/images/worklog/week-2/16.png)
  * Confirmation of successful NAT Gateway creation:
![NAT Gateway Successfully Created](/images/worklog/week-2/17.png)
  * Confirmation of successful Private Route Table creation:
![Private Route Table Successfully Created](/images/worklog/week-2/18.png)
  * Confirmation of successful Private Subnet associations:
![Private Subnet Associations Successful](/images/worklog/week-2/19.png)
  * Confirmation of Route configuration (0.0.0.0/0 pointing to NAT Gateway):
![Route Configuration Confirmed](/images/worklog/week-2/20.png)

#### 9. Setting Up EC2 Instance Connect Endpoint (Optional)
- **Description:** EC2 Instance Connect Endpoint (EIC Endpoint) is a secure connection solution that allows access to EC2 instances in a Private Subnet without requiring a Public IP address or a Bastion host. This solution reduces management costs and enhances system security.
- **Operations:** 
    - **Create Security Group for EIC Endpoint:** Navigated to Security Groups and clicked **Create security group**. Configured Name: **EIC Endpoint**, Description: **Allow SSH for MyIP**, and selected **ASG VPC**.
    - **Deploy EIC Endpoint:** 
        - Navigated to the VPC Dashboard, selected **Endpoints**, and clicked **Create endpoint**.
        - Configured Name: **EC2 private endpoint**.
        - Service category: Selected **EC2 Instance Connect Endpoint**.
        - VPC: Selected **ASG-VPC**.
        - Security group: Selected the newly created **EIC Endpoint**.
        - Subnet: Selected **Private subnet 2**.
        - Clicked **Create endpoint** and waited for the status to change to **Available**.

- **Image:**
  * Successful configuration of EC2 Instance Connect Endpoint:
![EIC Endpoint Configuration Successful](/images/worklog/week-2/21.png)
+
+#### 10. Shell Access via AWS Systems Manager Session Manager
+- **Description:** AWS Systems Manager Session Manager provides secure, browser-based interactive shell access to EC2 instances without the need for SSH keys, bastion hosts, or open inbound ports in Security Groups. This solution implements a "Zero Trust" security model and centralized access management.
+- **Operations:** 
+    - **Create IAM Role:** Initialized the **EC2-SessionManager-Role** with the **AmazonSSMManagedInstanceCore** policy.
+    - **Attach IAM Role:** Performed **Modify IAM role** for both Public and Private EC2 instances to assign the newly created role.
+    - **Create VPC Endpoints for SSM:** Configured three required endpoints (**ssm**, **ssmmessages**, **ec2messages**) within the ASG VPC, attached them to the Private Subnets, and used the dedicated Security Group for Endpoints.
+    - **Using Session Manager:** Accessed the Systems Manager Console, selected **Session Manager**, and clicked **Start session** to enter the server terminal directly in the browser.
+    - **Security Group Optimization:** Completely removed the rules allowing SSH (Port 22) to enhance security.
+- **Image:**
+  * IAM Role creation with SSM Core policy:
+![IAM Role for SSM Creation](/images/worklog/week-2/22.png)
+  * Successful Session Manager connection interface to an EC2 Private instance:
+![Session Manager Connection Successful](/images/worklog/week-2/23.png)
+
+#### 11. Monitoring and Alerting with Amazon CloudWatch
+- **Description:** Implemented a comprehensive monitoring solution for the VPC infrastructure using CloudWatch to track network performance, detect security incidents, and optimize resources through automated alarms and email notifications (SNS).
+- **Operations:** 
+    - **NAT Gateway Monitoring:** Configured tracking for essential metrics such as **BytesInFromDestination**, **PacketDropCount**, and **ActiveConnectionCount**.
+    - **Configure CloudWatch Alarms:** Created alarms for excessive packet drops (**PacketDropCount**) and integrated them with **Amazon SNS** for automated alerting.
+    - **VPC Flow Logs Analysis:** Established **Metric Filters** to monitor rejected connections (**REJECT**), helping identify unauthorized access attempts early.
+    - **Dashboard Setup:** Created a centralized **VPC-Monitoring-Dashboard** to visually monitor the health and status of the entire VPC system.
+- **Image:**
+  * CloudWatch Alarm configuration for NAT Gateway and SNS integration:
+![CloudWatch Alarm Configuration](/images/worklog/week-2/24.png)
+  * Centralized VPC Monitoring Dashboard overview:
+![VPC Monitoring Dashboard Overview](/images/worklog/week-2/25.png)

#### 12. Configuring AWS Site-to-Site VPN (Branch Office Network)
- **Description:** Established a secure Site-to-Site VPN connection between the main office (**VPC ASG**) and a branch office (**VPC ASG VPN**). The goal is to enable EC2 instances across different networks to communicate securely via private IPs through an encrypted VPN tunnel.
- **Operations:** 
    - **Initialize Branch VPC:** Created a new VPC named **ASG VPN** with a CIDR block of `10.11.0.0/16`.
    - **Set Up Subnet:** Created the **VPN Public** subnet (`10.11.1.0/24`) and enabled **Auto-assign Public IP**.
    - **Internet Configuration:** Created and attached the **Internet Gateway VPN** to the branch VPC to ensure management connectivity.
    - **Routing:** Created the **Route table VPN - Public**, configured a route for `0.0.0.0/0` pointing to the IGW, and associated it with the subnet.
- **Image:**
  * Successful initialization of VPC ASG VPN for the branch network:
![VPC ASG VPN Created Successfully](/images/worklog/week-2/26.png)
  * Successful configuration of VPN Public Subnet and Auto-assign Public IP:
![VPN Public Subnet Created Successfully](/images/worklog/week-2/27.png)
  * Confirmation of Internet Gateway attachment to the VPN VPC:
![IGW VPN Attached Successfully](/images/worklog/week-2/28.png)
  * Route Table configuration and Subnet association for the VPN network:
![VPN Route Table Configured Successfully](/images/worklog/week-2/29.png)

+
+










