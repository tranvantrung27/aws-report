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

##### 2. Practical Implementation Steps
###### **Step 1: Resource Provisioning (S3 & EC2)**
* **Create S3 Bucket:**
  * Created an S3 bucket named `s3-instancestoragegw-trantrung04` in the `ap-southeast-1` (Singapore) region.
  ![S3 Storage Gateway Created](/images/worklog/week-11/s3_storagegw_created.png)
* **Launch EC2 Storage Gateway Instance:**
  * Launched an EC2 instance to simulate the on-premises Storage Gateway Appliance.
  * Instance Name: `StorageGatewayInstance`.
  * **AMI:** Community AMI `aws-storage-gateway-FILE_S3-2.1.1` (Image ID: `ami-014cf7b9e443b99c8`).
  * Due to student account limits, selected Instance type: `t3.small`.
  * Key pair: `fcj-key`.
  * **Storage (Volumes):** Added a second volume (EBS Volume) of **`150 GiB`** (`gp3` type) to serve as local cache storage.
  * The EC2 instance successfully launched and reached **Running** state:
  ![Launch Storage Gateway EC2 Success](/images/worklog/week-11/11_launch_storagegw_ec2_success.png)
  ![Storage Gateway EC2 Running](/images/worklog/week-11/12_storagegw_ec2_running.png)

###### **Step 2: Storage Gateway Configuration (Pending)**
* **Note:** When proceeding to the **Storage Gateway Console** to connect the Gateway Appliance with AWS S3, the system blocked access with a "Complete your account setup" verification page. The remaining steps (activating the gateway, creating File Share, and mounting SMB/NFS) are pending and will be completed as soon as AWS support fully verifies the account.

#### AWS Web Application Firewall (WAF)
##### 1. Introduction
Use AWS WAF to protect web applications from common network attacks (like SQL Injection, Cross-Site Scripting) and manage traffic based on custom rules (IP, geolocation).

##### 2. Implementation Steps
###### **Step 1: Deploy the sample Web App**
* Launch the `WAFSampleWebApp` EC2 instance to host the **AWS WAF Security Dashboard** web application.
* The web app successfully displays security status when accessed via the EC2 instance's public IP address:
![Web App Deployed Success](/images/worklog/week-11/15_deploy_web_app_success.png)

###### **Step 2: Create Web ACL and IP Set (BlacklistIP)**
* Create an **IP Set** named `BlacklistIP` in the `ap-southeast-1` region containing the IP address to be blocked (e.g., the user's personal public IP `115.73.31.31/32` for test verification).
![IP Set Added Personal IP](/images/worklog/week-11/19_ip_set_added_personal_ip.png)
* Initialize a **Web ACL** named `WAFWebACL` in the Singapore region:
![Create Web ACL Success](/images/worklog/week-11/16_create_web_acl_success.png)

###### **Step 3: Add Custom Rule to Block IP Set**
* Add a custom rule named `BlockIPSetRule` linked to the `BlacklistIP` IP Set to perform a **Block** action on all traffic coming from these IP addresses.
![Add Custom Rule Success](/images/worklog/week-11/18_add_custom_rule_success.png)

###### **Step 4: Configure Application Load Balancer (ALB)**
* Since AWS WAF cannot be attached directly to an EC2 instance, create a **Target Group** named `waftg` targeting port 80 of the `WAFSampleWebApp` instance.
* Launch an Internet-facing **Application Load Balancer** named `WAF-ALB` operating in 2 Availability Zones (`ap-southeast-1a`, `ap-southeast-1b`) and forwarding traffic to `waftg`.
![Create ALB Success](/images/worklog/week-11/20_create_alb_success.png)

###### **Step 5: Associate WAF with Load Balancer**
* Go to `WAFWebACL` -> **Associated AWS resources** tab -> Associate the newly created Application Load Balancer `WAF-ALB` to apply WAF protection rules.
![Associate ALB to WAF](/images/worklog/week-11/21_associate_alb_to_waf.png)

###### **Step 6: Test IP Blocking (Verify WAF Protection)**
* Accessing the web application directly via the EC2 instance's Public IP remains successful.
* However, attempting to access it via the Load Balancer's DNS name: `http://WAF-ALB-508958680.ap-southeast-1.elb.amazonaws.com`
* The request is instantly identified and blocked by AWS WAF, returning a **`HTTP 403 Forbidden`** error:
```bash
$ curl -I http://WAF-ALB-508958680.ap-southeast-1.elb.amazonaws.com
HTTP/1.1 403 Forbidden
Server: awselb/2.0
Date: Thu, 09 Jul 2026 15:37:41 GMT
Content-Type: text/html
Content-Length: 134
Connection: keep-alive
```

#### Manage Resources Using Tags and Resource Groups
##### 1. Introduction
Practice assigning Tags to resources for easier classification, cost management, and create Resource Groups to group resources together for operational and monitoring purposes.

##### 2. Detailed Implementation Steps
###### **Step 1: Assign Tags to the EC2 Instance**
* Go to the EC2 Management Console -> Select the `WAFSampleWebApp` instance -> Click the **Tags** tab -> Select **Manage tags**.
* Add the following new tags to classify the resources used in the workshop:
  * Key: `Environment` | Value: `Workshop`
  * Key: `Project` | Value: `WAF`
* Click **Save** to apply the tags. The console confirms successful tag updates:
![EC2 Tags Saved Success](/images/worklog/week-11/22_ec2_tags_saved_success.png)

###### **Step 2: Initialize a Tag-based Resource Group**
* Go to the **AWS Resource Groups & Tag Editor** service -> Select **Create resource group**.
* Configure the grouping parameters as follows:
  * **Group type:** Select `Tag based`
  * **Grouping criteria:**
    * **Resource types:** Leave blank or select `All supported resource types` to include all resource categories.
    * **Tags:** Enter the Tag Key `Environment` and Tag Value `Workshop` -> Click **Add**.
* Click **Preview group resources** to let the system scan for matching resources. The scanner successfully displays 1 EC2 instance `WAFSampleWebApp` (ID: `i-0295122e688cbab63`).
* **Group details:**
  * **Group name:** Enter `WAF-Workshop-Resources`.
  * **Group description:** `Resource Group for WAF Workshop Lab 27`.
* Click **Create group** to complete the setup. The system confirms successful creation of the Resource Group:
![Resource Group Created Success](/images/worklog/week-11/23_resource_group_created_success.png)

