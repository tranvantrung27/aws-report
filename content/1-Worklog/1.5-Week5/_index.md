---
title: "Week 5 Worklog"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Week 5 Objectives:

* Understand the working mechanism of EC2 Auto Scaling: Launch Template, Scaling Policies, Cooldown Period.
* Master the role of Application Load Balancer in traffic distribution and high availability.
* Distinguish between 4 Auto Scaling strategies: Manual, Scheduled, Dynamic, Predictive.
* Understand the importance of AWS cost management and how to use AWS Budgets to control spending.
* Practice deploying the FCJ Management application using Amazon EC2 Auto Scaling and Elastic Load Balancing.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ---- | ---------- | --------------- | ------------------ |
| 2   | - Study EC2 Auto Scaling theory: Launch Template, Scaling Policies, Cooldown Period <br> - Research 4 Auto Scaling strategies: Manual, Scheduled, Dynamic, Predictive | 05/18/2026 | 05/18/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - Study Application Load Balancer (ALB): mechanism, Target Group, Health Check <br> - Research AWS Budgets and AWS cost management | 05/19/2026 | 05/19/2026 | <https://docs.aws.amazon.com/> |
| 4   | - **Practice:** <br>&emsp; + Create VPC with public/private subnets across multiple AZs <br>&emsp; + Configure Auto-assign Public IP for Public Subnets <br>&emsp; + Create Security Group for FCJ Management application (FCJ-Management-SG) <br>&emsp; + Create Security Group for Database Instance (FCJ-Management-DB-SG) | 05/20/2026 | 05/20/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - **Practice:** <br>&emsp; + Launch EC2 instance as web server <br>&emsp; + Launch Database instance with Amazon RDS <br>&emsp; + Install data for the database | 05/21/2026 | 05/22/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **Practice:** <br>&emsp; + Deploy web server and configure FCJ Management application <br>&emsp; + Prepare metrics for Predictive Scaling | 05/23/2026 | 05/23/2026 | <https://cloudjourney.awsstudygroup.com/> |


### Week 5 Achievements:

#### Theory:

* Understood the working mechanism of EC2 Auto Scaling: Launch Template, Scaling Policies, Cooldown Period.

* Mastered the role of Application Load Balancer in traffic distribution and high availability.

* Distinguished between 4 Auto Scaling strategies: Manual, Scheduled, Dynamic, Predictive.

* Understood the importance of AWS cost management and how to use AWS Budgets to control spending.

#### Practice:

##### 1. Network Infrastructure Setup (VPC)

Lab overview: Deploying the **FCJ Management** application using **Amazon EC2 Auto Scaling** combined with **Elastic Load Balancing** to ensure high availability and flexible scalability.

**AWS Services used:**
* **Amazon VPC**: Creates an isolated virtual network environment for deploying AWS resources.
* **Amazon EC2**: Provides virtual servers to run the FCJ Management application.
* **Amazon RDS**: Managed relational database service for storing application data.
* **Amazon EC2 Auto Scaling**: Automatically adjusts the number of EC2 instances based on actual demand.
* **Elastic Load Balancing (ELB)**: Distributes incoming traffic across multiple EC2 instances.
* **Amazon CloudWatch**: Monitors resources and applications, collects and tracks metrics.
* **AWS Systems Manager**: Manages configuration and automates tasks on EC2 instances.

**Create VPC (AutoScaling-Lab):**

In the **Create VPC** interface, configure the following:
* Select **VPC and more**
* **Name**: `AutoScaling-Lab`
* **IPv4 CIDR block**: `10.0.0.0/16`
* Enable **Auto-assign public IPv4 address** for Public Subnets

![Configure Auto-assign Public IP for Public Subnets](/images/worklog/week-5/1.png)

##### 2. Create Security Group for FCJ Management Application

Configure the Security Group:
* **Security group name**: `FCJ-Management-SG`
* **Description**: `Security Group for FCJ Management`
* **VPC**: Select the VPC just created: `AutoScaling-Lab`

![Successfully created FCJ-Management-SG Security Group](/images/worklog/week-5/2.png)

##### 3. Create Security Group for Database Instance


Configure the Security Group:
* **Security Group name**: `FCJ-Management-DB-SG`
* **Description**: `Security Group for DB instance`
* **VPC**: Select the VPC just created
* **Inbound rules**:
  * Protocol: `MYSQL/Aurora` — Port: `3306`
  * Source: `FCJ-Management-SG`

![Successfully created FCJ-Management-DB-SG Security Group](/images/worklog/week-5/3.png)

##### Infrastructure Setup Summary:

Completed the basic network infrastructure setup including:
* **VPC and Subnets**: Isolated virtual network with subnets distributed across multiple Availability Zones
* **Internet Gateway**: Enables connectivity from VPC to the internet
* **Route Table**: Configured routing for subnets
* **Security Groups**: Established security rules for EC2 instances and RDS database

##### 4. Launch EC2 Instance

**Create EC2 Instance**

Access the AWS Management Console:
1. Search for and select the **EC2** service.
2. **Basic Configuration:** Set the instance name to `FCJ-Management`.
3. **Select Amazon Machine Image (AMI):** Choose Quick Start -> Amazon Linux -> **Amazon Linux 2023 AMI**.
4. **Select Instance Type:** Choose `t2.micro`.
5. **Create Key Pair:** Select *Create new key pair* -> Name it `fcj-key`, key pair type RSA, private key format `.pem` -> Click *Create key pair*.
6. **Network Settings:** Click *Edit*, select the created VPC `AutoScaling-Lab`, choose the Public subnet, and ensure Auto-assign public IP is enabled.
7. **Security Configuration:** Select *Select existing security group*, then choose `FCJ-Management-SG`.
8. Click **Launch instance**.

![Instance launched successfully](/images/worklog/week-5/4.png)

**Connect to EC2 Instance via SSH**

After the EC2 instance is successfully launched and in the "Running" state, proceed to connect:

*For Windows users:*
1. Use **PuTTYgen** to convert the `.pem` file to `.ppk`: Open PuTTYgen -> Load the `fcj-key.pem` file -> Select *Save private key* and save it.

![Successfully created .ppk file](/images/worklog/week-5/5.png)

2. Open **PuTTY** and configure:
   * Enter the **Public IP** of the EC2 instance in the *Host Name* field.
   * Under `Connection > SSH > Auth`, select *Browse* and navigate to the created `.ppk` file.
   * Enter `ec2-user` when prompted for a username.

![Successfully connected to EC2 Instance](/images/worklog/week-5/6.png)

##### 5. Launch Database Instance with Amazon RDS

**Create DB Subnet Group for Amazon RDS**

Access the AWS Management Console:
1. Search for and select the **RDS** service.
2. Select **Subnet groups** -> Click **Create DB subnet group**.
3. **Configure DB subnet group:**
   * **Name**: Enter `FCJ-Management-Subnet-Group`.
   * **Description**: Enter `Subnet Group for FCJ Management`.
   * **VPC**: Select the created VPC (`AutoScaling-Lab`).
   * Select the corresponding Availability Zones and subnets.

![Successfully created DB Subnet Group across 2 AZs](/images/worklog/week-5/7.png)

**Create Amazon RDS Database Instance**

Access the Amazon RDS Console:
1. Select **Databases** -> Click **Create database**.
2. **Database creation method:** Choose **Standard create**.
3. **Engine options:** Select **MySQL**.
   4. **Templates:** Choose **Production** -> Select **Multi-AZ DB instance**.
   5. **Settings:**
   * **DB instance identifier**: Enter `fcj-management-db-instance`.
   * **Master username**: Enter `admin`.
   * Switch to **Self managed**.
   * **Master password**: Enter a secure password (e.g., `123Vodanhphai`) and confirm.
   6. **Instance configuration:** Choose `db.m5d.large`, Storage type `General Purpose SSD (gp3)` with `20 GB` Allocated storage.
7. **Connectivity:**
   * Select **Don’t connect to an EC2 compute resource**.
   * **VPC**: Select `AutoScaling-Lab`.
   * **Subnet group**: Select the created subnet group.
   * **VPC security group**: Choose **Choose existing** -> Select `FCJ-Management-DB-SG`.
   8. **Initial Database:** In Additional configuration, enter the Initial database name as `awsfcjuer`.
9. Click **Create database**. The initialization process may take 5-10 minutes.

After successful creation, record the **Endpoint** and **Port** for application configuration.

**Database Setup Summary:**
* **Create DB Subnet Group**: Allocate the database across multiple Availability Zones.
* **Configure DB Instance**: Select the appropriate database type, version, and configuration.
* **Security Setup**: Apply specialized Security Group to control access.
* **Initialize Database**: Create the initial database for the application.

##### 6. Create Launch Template

**AMIs and Launch Template**

**Create Amazon Machine Images (AMIs) from EC2**

In the EC2 management console interface, on the left navigation pane:
1. Select **Instances** -> Select the `FCJ-Management` instance.
2. Select **Actions** -> Select **Image and templates** -> Click **Create image**.
3. In the *Create AMI* configuration panel, fill in the following information:
   * **Image name**: `FCJ-Management-AMI`
   * **Image description**: `AMI for FCJ-Management`
4. Click **Create Image**.

After creating the AMI, verify it:
* Select **AMIs** in the left navigation pane.
* Search for and select `FCJ-Management-AMI`.

![AMI created successfully](/images/worklog/week-5/8.png)


**Create Launch Templates**

In the EC2 management console interface, on the left navigation pane:
1. Select **Launch Templates** -> Select **Create launch template**.
2. In the *Create launch template* panel, fill in the following information:
   * **Launch template name**: `FCJ-Management-template`
   * **Template version description**: `Template for FCJ Management`
3. Under **Application and OS Image (Amazon Machine Image)**:
   * Select the **My AMIs** tab -> Select **Owned by me**.
   * Choose the created AMI `FCJ-Management-AMI`.
4. Under **Instance type**: Select `t2.micro`.
5. Under **Key pair (login)**: Select the Key pair name `fcj-key`.
6. Under **Network settings**:
   * Select the public subnet `AutoScaling-Lab-public-ap-southeast-1a`.
   * Select **Select existing security group**.
   * Choose the security group `FCJ-Management-SG`.
   7. Finally, select **Create launch template**.

Verify the created Launch Template:
![Launch Template created successfully](/images/worklog/week-5/9.png)


**Advanced customizations for Launch Template**

In addition to basic configurations, Launch Templates allow you to customize many advanced parameters to optimize performance and costs for EC2 instances in an Auto Scaling Group.
*Using User Data*: User Data is a powerful feature that allows you to run scripts when an instance starts.

##### 7. Create Target Group

**Create Target Group**

In the EC2 management console interface, on the left navigation pane, scroll down to the **Load Balancing** section:
1. Select **Target Groups** -> Select **Create target group**.
2. In the *Specify group details* panel, configure as follows:
   * Under **Basic configuration**:
     * **Choose a target type**: `Instances`
     * **Target group name**: `FCJ-Management-TG`
     * **Protocol**: `HTTP`
     * **Port**: `5000`
     * **IP address type**: `IPv4`
     * **VPC**: `AutoScaling-Lab`
     * **Protocol version**: `HTTP1`
3. Click **Next**.


**Register targets**:
1. Under **Available instances**:
   * Select the `FCJ-Management` instance.
   * **Ports for the selected instances**: `5000`
   * Select **Include as pending below**.
2. Under **Review targets**:
   * Verify the registered target.
   * Select **Create target group**.


**Result**
We have completed creating the Target Group. Select the newly created `FCJ-Management-TG` to view its details:

![Target Group Details](/images/worklog/week-5/10.png)

##### 8. Create Load Balancer

**Create Application Load Balancer**

In the EC2 management console interface, on the left navigation pane:
1. Select **Load Balancers** -> Click the **Create Load Balancer** button.
2. In the *Compare and select load balancer type* panel:
   * Under **Load balancer types**:
   * At the **Application Load Balancer** section, click **Create**.


In the *Create Application Load Balancer* panel:
1. Under **Basic configuration**:
   * **Load balancer name**: `FCJ-Management-LB`
   * **Scheme**: `Internet-facing`
   * **Load balancer IP address type**: `IPv4`
2. Under **Network mapping**:
   * **VPC**: `AutoScaling-Lab`
   * **Subnet**: Select Public Subnets in `ap-southeast-1a`, `ap-southeast-1b`, and `ap-southeast-1c`.
   3. Under **Security groups**:
   * Select `FCJ-Management-SG`
4. Under **Listeners and routing**:
   * **Default action**: `FCJ-Management-TG`
   5. Under **Summary**, verify the configured information for the Load Balancer.
6. Click the **Create Load Balancer** button.

**Verify and check Load Balancer**
After creating the Load Balancer, select `FCJ-Management-LB` to view its details:

![Load Balancer created successfully](/images/worklog/week-5/11.png)

##### 9. Verify Results

**Verify Deployment Results**

To access the application, use the Load Balancer's **DNS name** and paste it into your web browser. 
The result will display our management application's website.


**Check System Stability**
To verify system stability, we will perform some basic operations. First, let's try modifying the information of a record in the application.
After entering the information, click **Submit** and observe the confirmation message:

![Confirmation Message](/images/worklog/week-5/12.png)

Returning to the homepage, we can see that the information has been successfully updated:

![Result after update](/images/worklog/week-5/13.png)



**Check System Load Capacity**

Access the **CloudWatch** service from the AWS Management Console. In CloudWatch, we can view Load Balancer metrics such as RequestCount, TargetResponseTime, and HTTPCode.


**Check Auto-healing Capability**
An important feature of architectures using Load Balancers and Auto Scaling Groups is the ability to self-heal when an instance fails. To test this feature:
1. Access the **EC2 Management Console**.
2. Select one of the instances running in the Auto Scaling Group.
3. Perform a **Terminate instance** operation.
4. After terminating the instance, the Auto Scaling Group will automatically detect the failure and launch a new instance as a replacement.


**Check Auto Scaling Capability**
An Auto Scaling Group allows the system to automatically scale out during high load. To test this feature, we can generate artificial load and observe the response:
* Use a load generation tool like Apache JMeter or AWS Load Testing.
* Monitor metrics like CPU Utilization in CloudWatch.
* Observe the Auto Scaling Group automatically increasing the number of instances when the load exceeds the threshold.


**Check Availability Zone Fault Tolerance**
A major benefit of deploying applications across multiple Availability Zones is fault tolerance when an AZ experiences an outage. To test this feature:
1. Access the **VPC Management Console**.
2. Select a subnet in a specific AZ where the application is running.
3. Temporarily disable traffic to that subnet by modifying Network ACLs.
4. Observe the Load Balancer automatically routing traffic to instances in the remaining healthy AZs.


##### 10. Create Auto Scaling Group

**Previous Issue**

**Setup Auto Scaling Group**

**Create Auto Scaling Group**
In the EC2 management console, scroll to the bottom of the left navigation pane:
1. Select **Auto Scaling Groups** -> Click **Create Auto Scaling group**.
2. In the Create Auto Scaling group interface, fill in the following:
   * **Name**: `FCJ-Management-ASG`
   * Under **Launch template**: Choose `FCJ-Management-template`, Version: `Default (1)`



**Network Settings**
Under the **Network** section, configure as follows:
* **VPC**: choose the previously created `AutoScaling-Lab` VPC.
* **Availability Zones and subnets**: select the 3 created public subnets.
* Click **Next**.

**Load Balancer Configuration**

* **Load balancing**: select `Attach to an existing load balancer`
* **Attach to an existing load balancer**: choose `Choose from your load balancer target group`
* **Existing load balancer target group**: choose `FCJ-Management-TG | HTTP`


* Under **VPC Lattice integration options**: select `No VPC Lattice service`.
* Next, under **Health checks**: Check `Turn on Elastic Load Balancing health checks`. Keep the remaining settings at default.
* Under **Additional settings**, in Monitoring: Check `Enable group metrics collection within CloudWatch`.
* Click **Next**.

**Size and Scaling Policies**

* Under **Group size**:
  * **Desired capacity**: `1`
* Under **Scaling limits**:
  * **Min desired capacity**: `1`
  * **Max desired capacity**: `3`
* Under **Automatic scaling - optional**: choose `No scaling policies` (we will not set up policies just yet).
* Under **Instance maintenance policy**: choose `No policy`.


**Notification Settings**

Configure notifications:
* **Send a notification to**: `asg-topic` (create a new topic)
* **With these recipients**: enter your email
* **Event types**: select all
* Click **Next**.

Review the information and click **Create Auto Scaling group**.

**Results**

![Email Subscription](/images/worklog/week-5/14.png)

![Confirm Subscription](/images/worklog/week-5/15.png)

Since we set Desired capacity = 1, the ASG will automatically create a new Instance, and you will receive a notification email:

![Instance Creation Email](/images/worklog/week-5/16.png)

Go to the Activity tab of the `FCJ-Management-ASG` to verify.

##### 11. Configure AWS Budget

**Overview**
In this section, we create an AWS Budget using a built-in template. AWS Budget is a tool that helps track and control AWS costs effectively.

**Create Budget from template**
Access the AWS Management Console:
1. Open the AWS Management Console.
2. Search for and select the **AWS Billing and Cost Management** service.

In the AWS Billing and Cost Management interface:
1. Select **Budgets** from the left menu.
2. Click on **Create a budget**.

Configure Budget settings:
1. Select **Use a template (simplified)** to use a pre-built template.
2. Under Templates, choose **Monthly cost budget**.

Enter Budget details:
1. Provide a name for the Budget.
2. Define the monthly budget amount.
3. Set alert thresholds.
4. Click **Create budget** to finish.

Verify that the Budget was created successfully:
![Budget created and verified successfully](/images/worklog/week-5/17.png)

View Budget history and check the available alert types in the template through the Budgets interface.

##### 12. Create Custom Cost Budget

**Initialize Cost Budget**
1. Log in to the AWS Management Console and search for **Billing and Cost Management**.
2. On the management page, select **Budgets** from the left menu. Click the **Create budget** button.

**Configure Budget Settings**
1. Under Budget setup:
   * Select **Customize**.
   * Under Budget types, select **Cost budget**.
   * Click **Next**.
2. In the Set your budget interface:
   * Under **Budget name**, enter `Monthly`.
   * **Period**: Select **Monthly**.
   * **Budget effective dates**: Select **Recurring Budget**.
   * **Specify your monthly budget**: Select **Fixed** and enter the desired amount in the **Budgeted amount** field.
3. Under Budget scope, select **All AWS services**. Under Aggregate costs by, select **Unblended costs**. Then click **Next**.

**Setup Alerts**
1. Select **Add an alert threshold** to configure alert thresholds.
2. Configure the Alert details (e.g., threshold percentage and notification email) and click **Next**.
3. Review action settings (if any) and click **Next**.
4. Review the entire configuration and click **Create budget** to finish.

**Verify that the Budget was created successfully:**

![Cost Budget created successfully](/images/worklog/week-5/18.png)

##### 13. Create Usage Budget

**Initialize Usage Budget**
1. Log in to the AWS Management Console and search for **Billing and Cost Management** in the search bar.
2. On the management page, select **Budgets** from the left menu. Click the **Create budget** button.

**Configure Budget Settings**
1. Under Budget setup:
   * Select **Customize**
   * Under Budget types, select **Usage budget**
   * Click **Next**
2. In the Set your budget interface:
   * Under **Budget name**, enter a name for your budget.
   * Under **Budget against**:
     * Select **Usage type groups**
     * Select **EC2: ELB - Running Hours** to track the running hours of EC2 instances
3. Configure Set budget amount:
   * **Period**: Select the budget period (Daily/Monthly/Quarterly/Annually)
   * **Budget renewal type**: Select the renewal type (Recurring/Expiring)
   * **Budgeting method**: Select the budgeting method (Fixed/Planned)
   * Enter the maximum number of usage hours you want to limit
4. Under Budget scope, keep the defaults and click **Next**.

**Setup Alerts**
1. Under Configure alerts, select **Add an alert threshold** to set alert thresholds.
2. Complete the Alert details:
   * Set the alert threshold (% of budget)
   * Add an email address to receive notifications when the threshold is exceeded
3. Click **Next** to continue.
4. Review the settings and click **Create budget** to finish.

**View and Manage Budget**
After successfully creating it, you will see the new budget in the list.

![View Budget List](/images/worklog/week-5/19.png)

You can check **Budget health** to monitor current usage against the set budget, and view **Budget history** to track usage trends over time.

##### 14. Create RI Budget

**Initialize Reservation Budget**
1. Log in to the AWS Management Console and select the **AWS Billing and Cost Management** service from the search bar.
2. On the management page, select **Budgets**. Click **Create budget**.

**Perform Budget Setup**
1. Select **Customize**
2. Select **Reservation budget**
3. Click **Next**
4. Set the budget name.
5. Configure Coverage threshold.
6. Configure Budget scope.
7. In the Alert setting section, enter an email address. Click **Next**.
8. Click **Create budget**.

**Complete Budget Creation**
Verify and check the newly created budget in the list.

![RI Budget created successfully](/images/worklog/week-5/20.png)

After successful creation, you will see a confirmation message. Check the details of the created budget in the budgets list.

![RI Budget Details](/images/worklog/week-5/21.png)
