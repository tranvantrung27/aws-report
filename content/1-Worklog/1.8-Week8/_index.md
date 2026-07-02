---
title: "Week 8 Worklog"
date: 2026-06-08
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Week 8 Objectives:

* Learn and practice migrating servers from On-premise to AWS using **VM Import/Export**.
* Containerize applications using **Docker** and manage containers with **Docker Compose** on **Amazon EC2**.
* Configure application containers to connect with **Amazon RDS** and store container images in **Amazon ECR** and **Docker Hub**.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | - Learn VM Import/Export concepts <br> - **Lab 14:** Prepare local VM and export from On-premise environment | 08/06/2026 | 08/06/2026 | [VM Import/Export Requirements](https://docs.aws.amazon.com/vm-import/latest/userguide/vmie_prereqs.html) |
| 3 | - **Lab 14:** Upload VM file to Amazon S3 <br> - Run CLI commands to import VM as AWS AMI and launch EC2 instance | 09/06/2026 | 09/06/2026 | [Importing a VM as an Image](https://docs.aws.amazon.com/vm-import/latest/userguide/vmimport-image.html) |
| 4 | - **Lab 14:** Configure S3 Bucket ACL <br> - Export EC2 Instance/AMI back to S3 as VM file and clean up resources | 10/06/2026 | 10/06/2026 | [Exporting an Instance/Image](https://docs.aws.amazon.com/vm-import/latest/userguide/vmexport.html) |
| 5 | - **Lab 15:** Deploy and test the application on Local environment <br> - AWS Preparation: Configure VPC, Security Group, IAM Roles for ECR, and log in to Docker Hub | 11/06/2026 | 11/06/2026 | [Docker Documentation](https://docs.docker.com/) <br> [Amazon ECR IAM Policies](https://docs.aws.amazon.com/AmazonECR/latest/userguide/security_iam_id-based-policy-examples.html) |
| 6 | - **Lab 15:** Create DB Subnet Group and launch RDS Instance <br> - Launch and configure EC2 Instance as Docker host and install dependencies | 12/06/2026 | 12/06/2026 | [Amazon RDS DB Instance Creation](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_CreateDBInstance.html) <br> [Docker on AWS EC2](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/docker-wrapper.html) |
| 7 - Sun | - **Lab 15:** Build Docker Image, run container and configure Docker Compose on EC2 to connect to RDS <br> - Push Docker Image to Amazon ECR and Docker Hub <br> - Clean up resources to prevent charges | 13/06/2026 | 14/06/2026 | [Docker Compose Reference](https://docs.docker.com/compose/) <br> [ECR Push Image Commands](https://docs.aws.amazon.com/AmazonECR/latest/userguide/docker-push-ecr-image.html) |

### Week 8 Achievements:

---

### Week 8 Practice Details:

#### Lab 1: VM Import/Export (Lab 14)
##### 1. Prepare VM
* Installed virtualization software **VMware Workstation Pro** in the local environment.
* Created a new **Ubuntu Desktop** virtual machine with a single-file virtual disk of **20 GB**.
* Enabled SSH server access on the VM for remote administration:
```bash
sudo apt update
sudo apt install openssh-server -y
sudo systemctl enable ssh --now
```

##### 2. Import VM into AWS
###### 2.1 Export VM from On-premise
* Powered off the virtual machine in VMware Workstation.
* Clicked **File** -> **Export to OVF...**
* Generated a virtual disk image file with the **`.vmdk`** extension (e.g. `Ubuntu-disk1.vmdk`).

###### 2.2 Upload VM to AWS
* Created a globally unique S3 Bucket `vm-import-export-bucket-trung-2026` with **ACLs enabled** and Block Public Access turned off.
* Uploaded the `Ubuntu-disk1.vmdk` file into the bucket.
* Verified the bucket configurations on the console:

![S3 ACL Enabled](/images/worklog/week-8/1_s3_acl_enabled.png)

###### 2.3 Import VM into AWS
* Configured a service role named `vmimport` to authorize the import service.
* Created the trust relationship file `trust-policy.json`:
```json
{
   "Version": "2012-10-17",
   "Statement": [
      {
         "Effect": "Allow",
         "Principal": { "Service": "vmie.amazonaws.com" },
         "Action": "sts:AssumeRole",
         "Condition": { "StringEquals": { "sts:ExternalId": "vmimport" } }
      }
   ]
}
```
* Created the IAM Role:
```bash
aws iam create-role --role-name vmimport --assume-role-policy-document "file://trust-policy.json"
```
* Configured S3 and EC2 policy permissions for the role `role-policy.json`:
```bash
aws iam put-role-policy --role-name vmimport --policy-name vmimport-policy --policy-document "file://role-policy.json"
```
* Verified `vmimport` role creation with correct Trusted Entity in IAM Console:

![IAM Role vmimport](/images/worklog/week-8/1_iam_role_vmimport.png)

* Created `containers.json` config and initiated the image import process via CLI:
```bash
aws ec2 import-image --description "Ubuntu App Server Import" --disk-containers "file://containers.json"
```
* Monitored the image import task to track progress until AMI generation:
```bash
aws ec2 describe-import-image-tasks --import-task-ids <ImportTaskId>
```

###### 2.4 Deploy EC2 Instance from AMI
* Located the imported AMI under EC2 Console > **AMIs**.
* Selected **Launch instance from AMI** to launch a `t3.micro` instance named `Import-Server` (configured with SSH port 22).
* SSHed into the instance from the local machine:
```bash
ssh awsstudent@<Public_IP_EC2>
```

##### 3. Export EC2 Instance from AWS
###### 3.1 Set up ACL for S3 Bucket
* Authorized the VM Import/Export service to write output files back to S3 by adding writing permissions to its Canonical ID:
```bash
aws s3api put-bucket-acl --bucket YOUR_BUCKET_NAME --grant-write "id=c4d8eabf8db69dbe46bfe0e517100c554f01200b104d59cd408e777ba442a322" --grant-read-acp "id=c4d8eabf8db69dbe46bfe0e517100c554f01200b104d59cd408e777ba442a322"
```

###### 3.2 Export VM from EC2 Instance
* Initiated instance export task back to S3 as a `.ova` package:
```bash
aws ec2 create-instance-export-task --instance-id <InstanceId> --target-environment vmware --export-to-s3-task DiskImageFormat=vmdk,ContainerFormat=ova,S3Bucket=YOUR_BUCKET_NAME,S3Prefix=exports/
```

###### 3.3 Export VM from AMI
* Executed image export task directly from the AMI:
```bash
aws ec2 export-image --image-id <AMI_ID> --disk-image-format VMDK --s3-export-location S3Bucket=YOUR_BUCKET_NAME,S3Prefix=exports/
```

##### 4. Reference Video
* Watched reference video demonstrations on vm import/export to grasp workflow patterns.

##### 5. Resource Cleanup
Cleaned up resources to prevent charges:
```bash
aws ec2 terminate-instances --instance-ids <InstanceId>
aws ec2 deregister-image --image-id <AMI_ID>
aws s3 rb s3://vm-import-export-bucket-trung-2026/ --force
aws iam delete-role --role-name vmimport
```

---

#### Lab 2: Deploy App with Docker / Docker Compose / RDS / ECR (Lab 15)
##### 1. Introduction
Lab 15 covers containerizing a Fullstack application (React frontend, Express backend, and MySQL database) with Docker, running it on EC2 using Docker Compose, storing persistent data in Amazon RDS MySQL, and pushing container images to AWS ECR and Docker Hub.

##### 2. Deploy Local
###### 2.1. Install Dependencies
Install essential dependencies locally including Git, Node.js & NPM, and MySQL Server:
```bash
git --version
node -v
npm -v
mysql --version
```

###### 2.2. Deploy Application
* Clone the sample repository:
```bash
git clone https://github.com/AWS-First-Cloud-Journey/aws-fcj-container-app.git
cd aws-fcj-container-app
```
* Import the database structure script `/database/init.sql` into local MySQL.
* Create a `.env` environment variables configuration file inside `/backend`:
```env
PORT=5000
DB_HOST=localhost
DB_USER=root
DB_PASSWORD=your_password
DB_NAME=fcjresbar
```
* Install dependencies and start Backend:
```bash
cd backend && npm install && npm run dev
```
* Install dependencies and start Frontend:
```bash
cd ../frontend && npm install && npm start
```

###### 2.3. Test Application
* Open browser at `http://localhost:3000` to verify the local deployment works properly.

##### 3. Preparation
###### 3.1. Configure VPC
* Created VPC `FCJ-Lab-vpc` with CIDR block `10.0.0.0/16`.
* Provisioned 1 Public Subnet (`10.0.1.0/24`) for EC2 Web Server, and 2 Private Subnets (`10.0.2.0/24`, `10.0.3.0/24`) in separate AZs for Amazon RDS.
* Configured an Internet Gateway (IGW) and attached a Route Table to allow Public Subnet access to the internet.

###### 3.2. Configure Security Group
Created 2 Security Groups to enforce traffic separation:
* `FCJ-Lab-sg-public` (EC2 host): Allows inbound ports 22 (SSH), 80 (HTTP), 3000 (Frontend) and 5000 (Backend) from any source (`0.0.0.0/0`).
* `FCJ-Lab-sg-private` (RDS DB): Allows inbound MySQL traffic on port 3306 exclusively from `FCJ-Lab-sg-public`.

###### 3.3. Create IAM Roles for ECR Access
* Created IAM Role `CustomRWECRRole` trusted by the EC2 service (`ec2.amazonaws.com`).
* Attached ECR read/write inline policy permissions (`ReadECRRepositoryContent` and `WriteECRRepositoryContent`).
* Verified ECR Role creation on IAM console:

![IAM Role for ECR](/images/worklog/week-8/2_iam_role_ecr.png)

###### 3.4. Log in to Docker Hub
* Created Docker Hub credentials for image upload.

##### 4. Launch RDS Instance
###### 4.1. Create DB Subnet Group
* Configured DB Subnet Group `fcj-lab-subnet-group-db` referencing both private subnets:
* Verified DB Subnet Group in RDS Console:

![DB Subnet Group](/images/worklog/week-8/2_rds_subnet_group.png)

###### 4.2. Launch RDS Instance
* Launched an Amazon RDS MySQL Instance `fcj-lab-rds-instance` on a `db.t3.micro` class, utilizing `fcj-lab-subnet-group-db` and assigning `FCJ-Lab-sg-private`.
* Verified RDS instance is successfully creating on Console:

![RDS Instance Creating](/images/worklog/week-8/2_rds_creating.png)

##### 5. Configure EC2 Instance
###### 5.1. Configure EC2 Instance
* Launched an EC2 Host `FCJ-Lab-my-server` in the Public Subnet and assigned the ECR role profile `CustomRWECRRole`.

###### 5.2. Install Required Libraries
SSHed into EC2 and installed Docker Engine, Unzip, MySQL Client, and AWS CLI v2:
```bash
sudo apt update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin mysql-client unzip
```

###### 5.3. Add Test Database
* Authenticated database connection from EC2 to RDS Endpoint:
```bash
mysql -h <RDS_ENDPOINT> -u admin -p
```
* Run sql script to build schema structures:
```sql
source aws-fcj-container-app/database/init.sql;
```

##### 6. Deploy with Docker Image
###### 6.1. Deploy Application
* Created a Docker bridge network:
```bash
sudo docker network create my-network
```
* Built backend container:
```bash
cd backend && sudo docker build . -t backend-image
sudo docker run -d -p 5000:5000 --network my-network --name backend backend-image
```
* Built frontend container:
```bash
cd ../frontend && sudo docker build . -t frontend-image
sudo docker run -d -p 3000:80 --network my-network --name frontend frontend-image
```

###### 6.2. Test Application
* Navigated to `http://<EC2_PUBLIC_IP>:3000` to confirm data is retrieved from RDS via API Backend.

##### 7. Deploy with Docker Compose
###### 7.1. Deploy Application
* Stopped and removed standalone containers.
* Run Docker Compose stack using `docker-compose.app.yml` config:
```bash
sudo docker compose -f docker-compose.app.yml up -d
```

###### 7.2. Test Application
* Monitored container stack statuses:
```bash
sudo docker compose -f docker-compose.app.yml ps
```

##### 8. Push Image
###### 8.1. Use ECR
* Created repositories `fcjresbar-fe` and `fcjresbar-be` on ECR.
* Authenticated Docker client with ECR Registry:
```bash
aws ecr get-login-password --region ap-southeast-1 | sudo docker login --username AWS --password-stdin <AWS_ACCOUNT_ID>.dkr.ecr.ap-southeast-1.amazonaws.com
```
* Tagged and pushed images:
```bash
sudo docker tag backend-image:latest <AWS_ACCOUNT_ID>.dkr.ecr.ap-southeast-1.amazonaws.com/fcjresbar-be:latest
sudo docker push <AWS_ACCOUNT_ID>.dkr.ecr.ap-southeast-1.amazonaws.com/fcjresbar-be:latest
```

###### 8.2. Use Docker Hub
* Logged in to Docker Hub registry and pushed images:
```bash
sudo docker login
sudo docker tag backend-image:latest <DOCKER_HUB_USERNAME>/fcjresbar-be:latest
sudo docker push <DOCKER_HUB_USERNAME>/fcjresbar-be:latest
```

##### 9. Resource Cleanup
Cleaned up all created services:
```bash
aws rds delete-db-instance --db-instance-identifier "fcj-lab-rds-instance" --skip-final-snapshot
aws rds delete-db-subnet-group --db-subnet-group-name "fcj-lab-subnet-group-db"
aws ec2 terminate-instances --instance-ids <InstanceId>
aws ec2 delete-security-group --group-id <PublicSGID>
aws ec2 delete-security-group --group-id <PrivateSGID>
aws ec2 delete-vpc --vpc-id <VpcId>
```
