---
title: "Week 9 Worklog"
date: 2026-06-15
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

### Week 9 Objectives:

* Understand and practice deploying applications on **Amazon ECS (Elastic Container Service)** using Fargate, ALB, and Cloud Map.
* Learn and configure automated **CI/CD** pipelines using **GitLab CI/CD**, **GitHub Actions**, and **AWS CodeBuild**.
* Configure container monitoring with **Amazon CloudWatch Container Insights** and log routing using **AWS Firelens**.
* Utilize **AWS Security Hub** to evaluate the overall security posture (Security Score) against AWS security standards.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | - Learn concepts of Amazon ECS, Fargate, Task Definitions, and ECS Cluster operations | 15/06/2026 | 15/06/2026 | [Amazon ECS Developer Guide](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/Welcome.html) |
| 3 | - **Lab 16 (Part 1):** Configure VPC network, Subnets, NAT Gateway, Security Groups, and push Docker images to ECR/Docker Hub | 16/06/2026 | 16/06/2026 | [AWS ECR User Guide](https://docs.aws.amazon.com/AmazonECR/latest/userguide/what-is-ecr.html) |
| 4 | - **Lab 16 (Part 2):** Create ECS Cluster, Task Definitions for Frontend/Backend, configure Target Groups, ALB, and launch ECS Services (Blue/Green for Backend & Rolling Update for Frontend) | 17/06/2026 | 17/06/2026 | [AWS ECS Blue/Green Deployments](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/deployment-type-blue-green.html) |
| 5 | - **Lab 17 (Part 1):** Integrate automated CI/CD using GitLab Runner (set up IAM roles, variables, run pipeline) and GitHub Actions | 18/06/2026 | 18/06/2026 | [GitLab CI/CD Docs](https://docs.gitlab.com/ee/ci/) <br> [GitHub Actions Docs](https://docs.github.com/en/actions) |
| 6 | - **Lab 17 (Part 2):** Set up AWS CodeBuild for Frontend/Backend; configure Container Insights (CloudWatch) and logs routing via AWS Firelens to S3 | 19/06/2026 | 19/06/2026 | [AWS CodeBuild User Guide](https://docs.aws.amazon.com/codebuild/latest/userguide/welcome.html) <br> [AWS Firelens Guide](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/using_firelens.html) |
| 7 - Sun | - **Lab 18:** Enable AWS Security Hub, activate security standards, analyze the Security Score, and clean up resources | 20/06/2026 | 21/06/2026 | [AWS Security Hub User Guide](https://docs.aws.amazon.com/securityhub/latest/userguide/what-is-securityhub.html) |

### Week 9 Achievements:

* **Amazon ECS (Fargate):** Successfully deployed a containerized architecture on ECS with independent Frontend and Backend services using Serverless compute.
* **Load Balancing & Routing:** Configured Application Load Balancers (ALB) to handle path-based routing, Blue/Green Deployment (via AWS CodeDeploy) for Backend, and Rolling Update for Frontend.
* **Multi-Platform CI/CD:** Designed automation workflows using self-hosted GitLab Runner on EC2, GitHub Actions workflow secrets, and AWS CodeBuild project configurations.
* **Monitoring & Log Routing:** Enabled CloudWatch Container Insights metrics and deployed AWS Firelens (Fluent Bit) logs router sidecar to store container stdout/stderr logs in Amazon S3.
* **Security Auditing:** Activated AWS Security Hub and obtained the security compliance score based on AWS Foundational Security Best Practices and CIS AWS Foundations Benchmark.

---

### Practice Details for Week 9:

#### Lab 1: Deploying Application to Amazon ECS (Lab 16)

##### 1. Introduction
In this lab, we containerize our multi-tier application and deploy it onto **Amazon ECS** using **AWS Fargate** (a serverless engine for containers). The application architecture consists of a Frontend and a Backend service communicating with each other. We configure an **Application Load Balancer** to handle request routing and implement **Blue/Green Deployment (Backend)** and **Rolling Update Deployment (Frontend)** strategies.

##### 2. Preparation

###### 2.1 Infrastructure Configuration
* Created a custom VPC named `FCJ-Lab-vpc` with CIDR block `10.0.0.0/16`.
* Set up all networking components including an Internet Gateway and a NAT Gateway to allow tasks in Private Subnets to pull base Docker images and packages from the internet.

###### 2.2 Create CodeDeploy Role
* Created an IAM Role named `ECS-CodeDeploy-Role` specifically for AWS CodeDeploy with the `AWSCodeDeployRoleForECS` policy.
* Configured the following trust relationship document:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "",
      "Effect": "Allow",
      "Principal": { "Service": "codedeploy.amazonaws.com" },
      "Action": "sts:AssumeRole"
    }
  ]
}
```
* Verified IAM Role trust relationships configuration on the console:

![IAM Role CodeDeploy](/images/worklog/week-9/1_iam_role_codedeploy.png)

###### 2.3 Add Subnet
Divided the VPC into Public and Private subnets across multiple Availability Zones for high availability:
* **Subnet Name**: `FCJ-Lab-subnet-private3`
  * IPv4 CIDR block: `10.0.32.0/20`
  * Availability Zone: ap-southeast-1a
* **Subnet Name**: `FCJ-Lab-subnet-private4`
  * IPv4 CIDR block: `10.0.64.0/20`
  * Availability Zone: ap-southeast-1b
* Verified subnet configurations on the console:

![VPC Subnets](/images/worklog/week-9/1_vpc_subnets.png)

###### 2.4 Configure NAT Gateway
* Created a NAT Gateway inside the Public Subnet, allocated an Elastic IP, and configured routing so tasks in the Private Subnets can securely access external endpoints.

###### 2.5 Configure Route Table
Configure routing rules:
* Public Route Table redirects `0.0.0.0/0` traffic to the Internet Gateway.
* Private Route Table redirects `0.0.0.0/0` traffic to the NAT Gateway.

###### 2.6 Configure Security Group
Create specific security groups:
* `ALB-SG`: Allows inbound HTTP (Port 80) traffic from anywhere (`0.0.0.0/0`).
* `ECS-Frontend-SG`: Allows inbound traffic only from `ALB-SG`.
* `ECS-Backend-SG`: Allows inbound traffic from `ECS-Frontend-SG` and `ALB-SG` on the API port.

##### 3. Prepare for Deployment

###### 3.1. Push image to ECR
Log in to Amazon ECR, create repositories for both frontend and backend, build Docker images locally, tag them, and push them to ECR:
```bash
# Login to ECR
aws ecr get-login-password --region ap-southeast-1 | docker login --username AWS --password-stdin <aws_account_id>.dkr.ecr.ap-southeast-1.amazonaws.com

# Build and push Image
docker build -t ecs-backend .
docker tag ecs-backend:latest <aws_account_id>.dkr.ecr.ap-southeast-1.amazonaws.com/ecs-backend:latest
docker push <aws_account_id>.dkr.ecr.ap-southeast-1.amazonaws.com/ecs-backend:latest
```

###### 3.2. Push image to Dockerhub
Log in to Docker Hub, tag the frontend container image, and push it to Docker Hub as a backup registry:
```bash
docker login -u <username>
docker tag ecs-frontend:latest <username>/ecs-frontend:latest
docker push <username>/ecs-frontend:latest
```

##### 4. Register namespace in Cloud Map
Set up **AWS Cloud Map** for private Service Discovery. Registered a namespace named `fcjresbar.internal` inside the VPC so ECS tasks can locate each other using internal DNS names.
* CLI Log output verifying successful registration:
```json
[
    {
        "Id": "ns-xpmew7oos26x4as6",
        "Arn": "arn:aws:servicediscovery:ap-southeast-1:938834038589:namespace/ns-xpmew7oos26x4as6",
        "Name": "fcjresbar.internal",
        "Type": "DNS_PRIVATE",
        "Description": "Private DNS for ECS Tasks"
    }
]
```

##### 5. Create ECS Cluster
* Navigate to the Amazon ECS Console and create a cluster named `FCJ-Lab-cluster`.
* Specified **Fargate** as the infrastructure provider to leverage serverless container execution.
* Verified active ECS Cluster on the console:

![ECS Cluster](/images/worklog/week-9/1_ecs_cluster.png)

##### 6. Create ECS Task Definition
Define Task Definitions describing the blueprint of containers (CPU, RAM, container image, environment variables, logs configuration).

###### 6.1. Backend task definition
* **Name**: `backend-task`
* **CPU / Memory**: `0.25 vCPU / 0.5 GB RAM`
* **Container Image**: ECR backend URI.
* **Port Mapping**: Map port `5000` for backend communication.
* **Log Configuration**: Enable awslogs to push stdout/stderr logs to CloudWatch Logs.

###### 6.2. Frontend task definition
* **Name**: `frontend-task`
* **CPU / Memory**: `0.25 vCPU / 0.5 GB RAM`
* **Container Image**: ECR or Docker Hub frontend URI.
* **Port Mapping**: Map port `80` or `3000` for client access.
* **Environment Variables**: Configure the API URL pointing to the backend's DNS address in Cloud Map: `backend.fcjresbar.internal`.

##### 7. Configure Application Load Balancer

###### 7.1. Create Target Group
Create IP-based target groups:
* `TG-Backend-Blue`: Port `5000`, target type `IP`.
* `TG-Backend-Green`: Port `5000`, target type `IP` (for Blue/Green deployment).
* `TG-Frontend`: Port `80`, target type `IP`.

###### 7.2. Create Application Load Balancer
Set up an internet-facing ALB named `ECS-ALB` in the Public Subnets with `ALB-SG`.
* Listener 1: Port `80` routing to `TG-Frontend`.
* Listener 2: Port `8080` routing to `TG-Backend-Blue` (with port `8081` configured for testing Green target).

##### 8. Create ECS Service

###### 8.1. Blue/Green Deployment and Service Scaling with Backend
Create the Backend ECS Service running on Fargate. Select **Blue/Green deployment type (powered by AWS CodeDeploy)**:
* Link the service to `ECS-ALB`.
* Map both `TG-Backend-Blue` and `TG-Backend-Green`.
* Configure an Auto Scaling policy to adjust the desired tasks count based on average CPU utilization.

###### 8.2. Deploy Rolling with Frontend
Create the Frontend ECS Service running on Fargate. Use the default **Rolling Update** deployment type:
* `Minimum healthy percent`: `100%`
* `Maximum percent`: `200%`
This configuration guarantees that healthy tasks remain active while replacement containers spin up.

##### 9. Test Result
* Enter the ALB DNS Name in your web browser.
* Verify that the Frontend interface loads successfully.
* Interact with the application to verify Frontend-Backend connectivity.
* Perform a dummy code update, trigger a CodeDeploy deployment, and verify that traffic transitions smoothly from Blue to Green.

##### 10. Clean Up Resources
Cleaned up all services to prevent costs:
```bash
aws ecs delete-cluster --cluster "FCJ-Lab-cluster"
aws servicediscovery delete-namespace --id "ns-xpmew7oos26x4as6"
aws iam delete-role --role-name ECS-CodeDeploy-Role
aws ec2 delete-subnet --subnet-id <PrivateSubnet3>
aws ec2 delete-subnet --subnet-id <PrivateSubnet4>
aws ec2 delete-vpc --vpc-id <VpcId>
```

---

#### Lab 2: CI/CD Automation & Monitoring (Lab 17)

##### 1. Introduction
This lab focuses on automating the delivery lifecycle of our containerized application using three CI/CD platforms: **GitLab CI/CD**, **GitHub Actions**, and **AWS CodeBuild**. We also implement monitoring utilizing **CloudWatch Container Insights** and establish custom container log routing via **AWS Firelens (Fluent Bit)**.

##### 2. Preparation
* Initialize git repositories for our application codebase on GitHub and GitLab.
* Set up IAM Users and Roles with ECR publish permissions and ECS Service update capabilities.

##### 3. CI/CD Deployment with GitLab

###### 3.1. Fork and Modify Repository
* Forked the template repository `fcj-lab/aws-fcj-container-app` to personal GitLab account.
* Modified `frontend/src/components/Menu.jsx` file, changing `"Dashboard"` to `"Dashboard v1.0.1"`.

###### 3.2. Install and Register GitLab Runner
Launched an EC2 Instance running Ubuntu Server and executed setup runner scripts:
```bash
# setup.sh
apt update -y
curl -L "https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.deb.sh" | sudo bash
apt install gitlab-runner -y
gitlab-runner --version
```
* Switched to `gitlab-runner` system user:
```bash
sudo -i
su gitlab-runner
cd
```
* Registered the runner instance with GitLab project:
```bash
gitlab-runner register --url https://gitlab.com --token <YOUR_REGISTRATION_TOKEN>
```
* Selected `shell` executor and run the runner daemon in background:
```bash
nohup gitlab-runner run > start-runner-be.txt 2>&1 &
```

###### 3.3. Configure Permissions and IAM Role
* Configured passwordless sudo privileges for the runner inside `/etc/sudoers` via `sudo visudo`:
```text
gitlab-runner ALL=(ALL) NOPASSWD:ALL
```
* Provisioned IAM Role `ECS-GitLabRunner-Role` and attached it as the instance profile to allow ECR and ECS updates.
* Verified IAM Role configuration on the console:

![IAM Role Runner](/images/worklog/week-9/1_iam_role_gitlab_runner.png)

###### 3.4. Configure Variables and Run Pipeline
* Set up CI/CD variables in GitLab settings: `CLUSTER_NAME`, `SERVICE_NAME_FE`, `SERVICE_NAME_BE`, `REGION`, `AWS_APPLICATION_NAME`, `AWS_DEPLOYMENT_GROUP_NAME`, `ACCOUNT_NUMBER`.
* Configured `.gitlab-ci.yml` pipeline file separating build, push, and deploy stages.

###### 3.5. Pipeline Execution Results
* Created and pushed a Git Tag. Monitored pipeline execution success in the GitLab CI/CD interface.
* Confirmed ECS task definition updated and the new version of frontend loaded successfully.

##### 4. CI/CD Deployment with GitHub Actions

###### 4.1. Clone GitHub Template
Clone the repository containing the GitHub workflow configuration template.

###### 4.2. Create a New Project and Push Code
Create a new GitHub repository, link your local workspace, and push the source code.

###### 4.3. Create Access key and Secret key
Set up repository secrets in Settings -> Secrets and variables -> Actions:
* `AWS_ACCESS_KEY_ID`
* `AWS_SECRET_ACCESS_KEY`
These credentials are used by the `.github/workflows/aws.yml` file to authenticate with AWS APIs.

###### 4.4. Check the Results
Monitor workflow runs under the **Actions** tab. Verify that GitHub Actions successfully pushes the image and triggers a deployment on Amazon ECS.

##### 5. CI/CD Deployment with CodeBuild

###### 5.1. GitHub Setup
Connect AWS CodeBuild to your GitHub account to authorize repository imports.

###### 5.2. Create Frontend CodeBuild
Create a build project named `Frontend-Build`. The build process is governed by the `buildspec.yml` file, compiling and packaging the Frontend container image.

###### 5.3. Create Backend CodeBuild
Create a build project named `Backend-Build` for the Backend service, building and pushing the container image to ECR.

###### 5.4. Create Tag
Configure a webhook filter in CodeBuild to trigger builds automatically when a Git Tag matching `v*.*.*` is pushed.

###### 5.5. Verify Results
Inspect CodeBuild execution logs to verify that container images are correctly pushed to ECR and the ECS task definitions are updated.

##### 6. Application Monitoring with Container Insights (CloudWatch)
Enable **CloudWatch Container Insights** on the ECS cluster `ECS-App-Cluster`. Monitor container performance metrics:
* CPU/Memory Utilization per task and service.
* Network network packets.
* Running tasks counts.
Create a custom CloudWatch Dashboard for visual metrics tracing.

##### 7. Logs Router with Firelens
AWS Firelens integrates Fluent Bit routing to direct container logs to third-party endpoints or AWS destinations.

###### 7.1. Create Amazon S3 Bucket for Logs Storage
* Provisioned S3 bucket `fcj-firelens-logs-trung-2026` to store container standard output logs.
* Verified S3 bucket creation on the console:

![S3 logs bucket](/images/worklog/week-9/1_s3_firelens_logs.png)

###### 7.2. Create IAM Role for ECS Task
* Created task role `ECSTaskFullAccessToS3Role` with policy permission `AmazonS3FullAccess`.

###### 7.3. Configure Firelens in Task Definition
* Configured log router sidecar container `amazon/aws-for-fluent-bit:stable` and modified main container `logConfiguration` driver to `awsfirelens`, targeting the S3 destination parameters.

###### 7.4. Verify Log Delivery
* Verified S3 log delivery directories are created automatically inside the destination bucket.

##### 8. Clean up resources
Delete CodeBuild projects, terminate GitLab Runner EC2 instances, empty and delete the logs S3 bucket, and disable Container Insights.

---

#### Lab 3: AWS Security Hub (Lab 18)

##### 1. Security Standards
**AWS Security Hub** automatically audits resource configurations against recommended standards:
* **AWS Foundational Security Best Practices (FSBP)**: Designed by AWS security experts.
* **CIS AWS Foundations Benchmark v1.2.0 & v1.4.0**: Consensus-based industry guidelines.

##### 2. Enable Security Hub
* Open the AWS Security Hub Console.
* Click **Enable Security Hub**.
* Enable both FSBP and CIS standards. Allow several hours for the initial scan to generate a report.

##### 3. Security Score by Standards
* View the compliance percentage (**Security Score**) on the Security Hub dashboard.
* Drill down into critical, high, medium, and low severity **Findings**.
* Follow the provided **Remediation** steps for failing checks (e.g., configuring MFA for the root account, narrowing wide Security Group rules, or blocking S3 public access).

##### 4. Clean Up Resources
Disable AWS Security Hub once report data is gathered to avoid ongoing monitoring costs.
