---
title: "Week 11 Worklog"
date: 2026-06-29
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

### Week 11 Objectives:

* Practice deploying a CI/CD pipeline using AWS CodePipeline, CodeCommit, CodeBuild, and CodeDeploy.
* Understand and configure AWS File Storage Gateway to connect with on-premises environments.
* Secure applications by configuring AWS Web Application Firewall (WAF).
* Manage AWS resources efficiently using Tags and Resource Groups.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                   | Start Date | Completion Date | Reference Material                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 2   | - **Practice: Deploy applications to EC2 with AWS CodePipeline (Part 1)** <br>&emsp; + Preparation (S3, Git, IAM roles) <br>&emsp; + Launch EC2 and install CodeDeploy Agent | 29/06/2026 | 29/06/2026 | [AWS CodeDeploy Guide](https://docs.aws.amazon.com/codedeploy/latest/userguide/welcome.html) |
| 3   | - **Practice: Deploy applications to EC2 with AWS CodePipeline (Part 2)** <br>&emsp; + Setup AWS CodeCommit, CodeBuild, CodeDeploy <br>&emsp; + Configure AWS CodePipeline for CI/CD automation | 30/06/2026 | 30/06/2026 | [AWS CodePipeline Guide](https://docs.aws.amazon.com/codepipeline/latest/userguide/welcome.html) |
| 4   | - **Practice: Using File Storage Gateway** <br>&emsp; + Create S3 Bucket, EC2 for Storage Gateway <br>&emsp; + Initialize Storage Gateway and File Shares <br>&emsp; + Mount File shares on On-premises machine | 01/07/2026 | 01/07/2026 | [AWS Storage Gateway](https://docs.aws.amazon.com/storagegateway/latest/userguide/WhatIsStorageGateway.html) |
| 5   | - **Practice: AWS Web Application Firewall (Part 1)** <br>&emsp; + Deploy the sample Web App <br>&emsp; + Use AWS WAF, configure Web ACLs and Managed rules | 02/07/2026 | 02/07/2026 | [AWS WAF Developer Guide](https://docs.aws.amazon.com/waf/latest/developerguide/waf-chapter.html) |
| 6   | - **Practice: AWS Web Application Firewall (Part 2)** <br>&emsp; + Create Custom rules to block IP/Countries <br>&emsp; + Enable Logging and Testing | 03/07/2026 | 03/07/2026 | [AWS WAF Managed Rules](https://docs.aws.amazon.com/waf/latest/developerguide/aws-managed-rule-groups.html) |
| 7 - Sun | - **Practice: Manage Resources Using Tags and Resource Groups** <br>&emsp; + Use Tags on Console and CLI <br>&emsp; + Create and manage Resource Groups | 04/07/2026 | 05/07/2026 | [Tagging AWS resources](https://docs.aws.amazon.com/tag-editor/latest/userguide/tagging.html) <br> [AWS Resource Groups](https://docs.aws.amazon.com/ARG/latest/userguide/welcome.html) |

### Week 11 Achievements:

* Mastered the CI/CD pipeline on AWS and integrated services like CodeCommit, CodeBuild, CodeDeploy, and CodePipeline to automate application deployments.
* Successfully understood and configured AWS File Storage Gateway to connect on-premises storage to AWS S3.
* Deployed AWS WAF to secure web applications against attacks using Web ACLs and custom rules.
* Learned how to effectively organize and manage AWS resources using Tags and Resource Groups.
* Successfully completed all 4 practical labs as guided.

---

### Week 11 Practical Details:

#### Deploy applications to EC2 with AWS CodePipeline

##### 1. Introduction
Practice building a complete CI/CD system using AWS Developer Tools to automatically deploy source code to EC2 instances. Instead of using CodeCommit (which is no longer available to new AWS accounts), this implementation integrates **GitHub** as the Source stage, coupled with **CodeBuild**, **CodeDeploy**, and orchestration managed by **CodePipeline**.

##### 2. Implementation Steps
###### **Step 1: Preparation**
* **Create S3 Bucket for Artifacts:**
  * Create the S3 bucket and configure the Bucket Policy to allow access.
  ![S3 Bucket Policy](/images/worklog/week-11/1_s3_bucket_policy.png)
  ![S3 Bucket List](/images/worklog/week-11/2_s3_bucket_list.png)
* **Create IAM Service Role for CodeDeploy:**
  * Create the IAM role named `CodeDeployServiceRoleEC2` with the Trust Policy allowing CodeDeploy service actions and attach the `AWSCodeDeployRole` policy.
  ![CodeDeploy Service Role](/images/worklog/week-11/4_codedeploy_service_role.png)
* **Create IAM Instance Profile for EC2:**
  * Create the IAM role named `EC2InstanceRoleForCodeDeployV2` with the use-case selected as **EC2** (to automatically generate a compatible Instance Profile).
  * Attach permissions policies: `AmazonS3ReadOnlyAccess` and `AmazonSSMManagedInstanceCore` to allow the EC2 instance to read build files from S3 and connect securely via Systems Manager.
  ![EC2 Instance Role V2](/images/worklog/week-11/6_create_role_v2_success.png)

###### **Step 2: CodeDeploy Agent & Launch EC2**
* Launch an EC2 instance with Amazon Linux 2023 OS in `ap-southeast-1` Singapore named `CodeDeployEC2`.
* Link the created Instance Profile: `EC2InstanceRoleForCodeDeployV2`.
* In the **User Data** section, input the shell script to automatically install and run the CodeDeploy Agent:
  ```bash
  #!/bin/bash
  sudo yum update -y
  sudo yum install -y ruby wget
  cd /home/ec2-user
  wget https://aws-codedeploy-ap-southeast-1.s3.ap-southeast-1.amazonaws.com/latest/install
  chmod +x ./install
  sudo ./install auto
  sudo systemctl enable codedeploy-agent
  sudo systemctl start codedeploy-agent
  ```
* EC2 instance successfully launched:
  ![Launch EC2 Success](/images/worklog/week-11/7_launch_ec2_success.png)

###### **Step 3: Setup Source Repository (GitHub)**
* Instead of CodeCommit, initialize a new public GitHub repository named `aws-fcj-pipeline`.
  ![GitHub Repo Created](/images/worklog/week-11/8_github_repo_success.png)
* Set up local files and push them:
  * `index.html`: Web page markup.
  * `appspec.yml`: Dictates file copies and hook script execution for CodeDeploy.
  * `buildspec.yml`: Outlines CodeBuild phases and artifacts collection.
  * `scripts/install_dependencies`: Shell script to install `httpd` (Apache server) prior to app deployment.
* Pushed all project source files successfully to GitHub.

###### **Step 4: AWS CodeBuild (Completed)**
* **Connect GitHub to CodeBuild:**
  * Access CodeBuild service in `ap-southeast-1` (Singapore) region.
  * Create a new Build project, select Source provider as **GitHub**.
  * AWS requires GitHub OAuth authentication - log in to GitHub account `tranvantrung27` and authorize AWS CodeBuild to access the repository.
  * Two-factor authentication (2FA) on GitHub via mobile device completed successfully.
  * GitHub OAuth connection successful, CodeBuild recognizes the account and repository.

* **Build Project `AWS-FCJ-APP` created successfully:**
  * **Source provider:** GitHub
  * **Repository:** `https://github.com/tranvantrung27/aws-fcj-pipeline.git` (using the GitHub repository created in Step 3)
  * **Environment:**
    * Operating System: Amazon Linux
    * Runtime: Standard
    * Image: `aws/codebuild/amazonlinux-x86_64-standard:5.0` (Standard Image 5.0)
    * Service role: Using the automatically created role `codebuild-AWS-FCJ-APP-service-role`
  * **Buildspec:** Uses the `buildspec.yml` file from the source code
  * **Artifacts:**
    * Type: Amazon S3
    * Bucket name: `aws-cicd-ec2-trantrung04`
  * **Logs:**
    * CloudWatch Logs: Enabled
    * Group name: `aws-cicd-ec2-group`
    * Stream name: `aws-cicd-ec2-stream`
  * Project created successfully, Configuration displayed in full on the Console.
  <!-- ![CodeBuild Project Created](/images/worklog/week-11/9_codebuild_project_created.png) -->

###### **Step 5: AWS CodeDeploy (Pending)**
* Create Application named `AWS-FCJ-APP` with Compute platform: **EC2/On-premises**.
* Create Deployment Group named `AWS-FCJ-DG`:
  * Service Role: Role with `AWSCodeDeployRole` policy (created in Step 1: `CodeDeployServiceRoleEC2`).
  * EC2 Instance: Select by tag Key=`Name`, Value=`CodeDeployEC2`.
  * Deployment Configuration: `CodeDeployDefault.OneAtATime`.
  * Load Balancer: Disabled.
* **Note:** At the time of this lab, the CodeDeploy Console redirects to the "Complete your account setup" page because the AWS account verification is still in progress. Need to wait for verification to complete before continuing.

###### **Step 6: AWS CodePipeline (Pending)**
* Create a Pipeline connecting: Source (GitHub `tranvantrung27/aws-fcj-pipeline`) -> Build (CodeBuild `AWS-FCJ-APP`) -> Deploy (CodeDeploy `AWS-FCJ-APP` / `AWS-FCJ-DG`).
* Test the automated deployment process when a new commit is pushed.
* CodePipeline Console accessed successfully (Pipelines page shows empty, no pipelines created yet).


#### Using File Storage Gateway
##### 1. Introduction
Practice creating a File Storage Gateway to connect local on-premises storage to Amazon S3 via traditional protocols like NFS/SMB.

##### 2. Implementation Steps
* **Step 1: Preparation**
  * Create an S3 bucket to serve as the backend storage.
  * Launch an EC2 instance to act as the Storage Gateway appliance (simulating an on-premises server).
* **Step 2: Create Storage Gateway**
  * Access the AWS Storage Gateway Console and create a new gateway of type **Amazon S3 File Gateway**.
  * Connect and activate the gateway using the IP address of the EC2 instance.
* **Step 3: Create File Shares**
  * Configure a File Share linking the Gateway to the created S3 bucket. 
  * Obtain the mount command (NFS/SMB) from the AWS Console.
* **Step 4: Mount File Share on On-premises machine**
  * Log into the client machine.
  * Execute the mount command to map the shared folder to the local file system and verify the automatic file synchronization to S3.

#### AWS Web Application Firewall (WAF)
##### 1. Introduction
Use AWS WAF to protect web applications from common network attacks (like SQL Injection, Cross-Site Scripting) and manage traffic based on custom rules (IP, geolocation).

##### 2. Implementation Steps
* **Step 1: Deploy the sample Web App**
  * Launch a sample Web Application via CloudFormation or manually deploy it as the target for the WAF.
* **Step 2: Web ACLs with managed rules**
  * Access AWS WAF and create a Web ACL (Access Control List).
  * Add AWS **Managed Rules** (e.g., *AWSManagedRulesCommonRuleSet*) to protect against common threats.
* **Step 3: Custom Rules**
  * Create basic and advanced **Custom Rules** to block requests from specific IP addresses or countries.
* **Step 4: Testing and Logging**
  * Attempt to access the website using a blocked IP to receive a `403 Forbidden` error.
  * Enable Logging to monitor blocked or allowed requests.

#### Manage Resources Using Tags and Resource Groups
##### 1. Introduction
Practice assigning Tags to resources for easier classification, cost management, and create Resource Groups to group resources together for operational and monitoring purposes.

##### 2. Implementation Steps
* **Step 1: Using Tags on Console**
  * Launch a resource (e.g., an EC2 instance) and assign tags like `Environment: Dev`, `Project: Workshop`.
  * Manage (add/remove) tags for multiple resources simultaneously.
* **Step 2: Filter resources by tag**
  * Use the Tag Editor tool to find all resources sharing a specific Tag.
* **Step 3: Using tags with CLI**
  * Use AWS CLI commands (e.g., `aws ec2 create-tags`) to assign tags or query resources directly from the terminal.
* **Step 4: Create a Resource Group**
  * Create a Tag-based Resource Group to automatically group all resources that satisfy the Tag conditions, making centralized management easier.
