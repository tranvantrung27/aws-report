---
title: "Week 4 Worklog"
date: 2026-05-11
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Week 4 Objectives:

* Learn about Amazon Relational Database Service (Amazon RDS) on AWS.
* Practice deploying network infrastructure (VPC, Security Group, Subnet Group) for the database.
* Initialize and configure an Amazon RDS database instance.
* Connect an application running on EC2 to the RDS database.
* Understand Backup, Restore mechanisms, and advanced features (Multi-AZ, Read Replicas).

### Tasks to be implemented this week:

| Day | Task | Start Date | End Date | Resource |
| --- | --- | --- | --- | --- |
| 2 | - Overview of Amazon RDS <br> - Compare RDS with other storage services (EC2 DB, DynamoDB, Redshift) | 11/05/2026 | 11/05/2026 | [Amazon RDS Documentation](https://docs.aws.amazon.com/rds/) |
| 3 | - Network setup for RDS: <br>&emsp; + Create VPC <br>&emsp; + Create Security Groups for EC2 and RDS <br>&emsp; + Create DB Subnet Group | 12/05/2026 | 12/05/2026 | [VPC for RDS](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_VPC.WorkingWithRDSInstanceinaVPC.html) |
| 4 | - Practice initializing an Amazon RDS instance <br> - Configure parameters and security options for the Database | 13/05/2026 | 13/05/2026 | [Creating an RDS DB Instance](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_CreateDBInstance.html) |
| 5 | - Deploy application on EC2 and connect to RDS <br> - Verify access and data management via Endpoint | 14/05/2026 | 14/05/2026 | [Connecting to RDS](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_GettingStarted.CreatingConnecting.MySQL.html) |
| 6 | - Learn and practice Backup & Restore <br> - Practice manual and automatic Snapshots | 15/05/2026 | 15/05/2026 | [RDS Backup and Restore](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_CommonTasks.BackupRestore.html) |
| 7-Sun | - Learn about Multi-AZ and Read Replicas <br> - Resource cleanup to optimize costs | 16/05/2026 | 17/05/2026 | [High Availability (Multi-AZ)](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.MultiAZ.html) |

### Week 4 Results:

#### Theoretical:
* Mastered Amazon RDS knowledge: Benefits, supported DB engines, management features.
* Understood how Multi-AZ and Read Replicas work.
* Learned to differentiate when to use RDS, DynamoDB, Redshift, or S3.
* Understood security mechanisms (Security Groups, Encryption) and backups (Snapshots) on RDS.

#### Practical:
* Successfully set up a secure network environment for the Database (DB Subnet Group, Security Groups).
* Successfully initialized an Amazon RDS instance (MySQL/MariaDB).
* Successfully connected a Web application on EC2 to the RDS database via Endpoint.
* Successfully performed data backup and restoration using Snapshots.
* Efficiently managed resources and performed cleanup to avoid unnecessary costs.

---

### Detailed Implementation - Week 4:

#### 1. Lab Overview
{{% notice info "Information" %}}
In this lab, we will deploy Amazon RDS (Relational Database Service) — a fully managed relational database service on AWS. We will set up a secure network environment, initialize a MySQL RDS instance, connect a Node.js application running on EC2 to RDS via Endpoint, and practice Backup & Restore using Snapshots.
{{% /notice %}}

{{% notice warning "Security Note" %}}
RDS instances should be placed in a **private subnet** and only allow connections from EC2 instances through Security Groups — Public access should be disabled to ensure security.
{{% /notice %}}

#### 2. Preparation Steps

##### 2.1 Create VPC
Create a VPC to set up an isolated network environment for EC2 and RDS resources.
![VPC created successfully](/images/worklog/week-4/1.png)
![VPC Configuration](/images/worklog/week-4/2.png)

##### 2.2 Create EC2 Security Group
Configure the Security Group for EC2 to allow SSH (port 22) and HTTP (port 5000 for the Node.js application).
![EC2 SG created successfully](/images/worklog/week-4/3.png)

##### 2.3 Create RDS Security Group
{{% notice warning "Security Note" %}}
The Security Group for RDS should be configured to only allow inbound traffic from the EC2 Instance Security Group on the DB engine port (e.g., 3306 for MySQL).
{{% /notice %}}
![RDS SG created successfully](/images/worklog/week-4/4.png)

##### 2.4 Create DB Subnet Group
{{% notice info "Information" %}}
A DB Subnet Group is a collection of subnets (usually private) that you create in a VPC and then assign to DB instances. Each DB Subnet Group should have subnets in at least two Availability Zones.
{{% /notice %}}
![DB Subnet Group created successfully](/images/worklog/week-4/5.png)

#### 3. Create EC2 instance
{{% notice info %}}
Amazon Elastic Compute Cloud (EC2) provides scalable computing capacity in the AWS Cloud. Using EC2 allows you to deploy virtual servers without investing in physical hardware.
{{% /notice %}}

**Steps to follow:**
1. Go to **EC2 Console** -> **Launch Instance**.
2. Set Instance Name (e.g., `App-Server`).
3. Select **AMI**: Amazon Linux 2023 (Free tier eligible).
4. Select **Instance type**: `t2.micro` or `t3.micro`.
5. Select **Key pair** for SSH connection.
6. Configure **Network settings**: Choose the created VPC and the EC2 Security Group.
7. Select **Launch instance**.

![EC2 initialization successful](/images/worklog/week-4/6.png)

**Connect via SSH using MobaXterm:**
- Use the public IP of the instance.
- Default User: `ec2-user`.
- Provide the `.pem` file in the Advanced SSH settings.

#### 4. Create RDS database instance
Create a MariaDB or MySQL DB Instance using Standard create.

1. Go to **RDS Console** -> **Create database**.
2. Select **Standard create**.
3. **Engine type**: MySQL (or MariaDB).
4. **Templates**: Free Tier (to avoid costs).
5. **Settings**:
    - DB instance identifier: `database-1`.
    - Master username: `admin`.
    - Master password: Configure a secure password.
6. **Connectivity**:
    - Select the corresponding VPC.
    - Select **Existing VPC security groups**: Choose the created RDS Security Group.
7. Select **Create database**.

#### 5. Application Deployment and RDS Connection

##### Install Git and Node.js on EC2
```bash
# Update system
sudo dnf update -y

# Install Git
sudo dnf install git -y

# Install NVM and Node.js
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
source ~/.nvm/nvm.sh
nvm install --lts
```

##### Deploy Application
1. Clone repository:
   ```bash
   git clone https://github.com/AWS-First-Cloud-Journey/AWS-FCJ-Management
   cd AWS-FCJ-Management
   ```
2. Install dependencies:
   ```bash
   npm install express dotenv express-handlebars body-parser mysql
   ```
3. Configure environment (`.env`):
   ```bash
   DB_HOST="RDS_ENDPOINT"
   DB_NAME="first_cloud_users"
   DB_USER="admin"
   DB_PASS="YourPassword"
   ```

##### Initialize RDS Data
Connect to RDS from EC2 and run the SQL script:
```sql
CREATE DATABASE IF NOT EXISTS first_cloud_users;
USE first_cloud_users;

CREATE TABLE `user` (
    `id` INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
    `first_name` VARCHAR(45) NOT NULL,
    `last_name` VARCHAR(45) NOT NULL,
    `email` VARCHAR(100) NOT NULL UNIQUE,
    `phone` VARCHAR(15) NOT NULL,
    `comments` TEXT NOT NULL,
    `status` ENUM('active', 'inactive') NOT NULL DEFAULT 'active'
) ENGINE = InnoDB;

-- Insert sample data
INSERT INTO `user` (`first_name`, `last_name`, `email`, `phone`, `comments`, `status`)
VALUES ('Amanda', 'Nunes', 'anunes@ufc.com', '012345 678910', 'I love AWS FCJ', 'active');
```

4. Start application:
   ```bash
   npm start
   ```
Access via browser: `http://<EC2-Public-IP>:5000`.

#### 6. Backup and Restore
{{% notice info "Information" %}}
Amazon RDS provides automated and manual backup features, allowing you to restore your database to a specific point in time or from a snapshot.
{{% /notice %}}

1. **Monitoring**: Check the **Monitoring** tab on the RDS console to view CPU, Connections, IOPS.
2. **Take Snapshot**: Select DB Instance -> **Actions** -> **Take snapshot**.
3. **Restore Snapshot**:
   - Select Snapshot -> **Actions** -> **Restore snapshot**.
   - Configure a new Instance and check the restoration status.

![Verify restored database](/images/worklog/week-4/7.png)
![Restore completed](/images/worklog/week-4/8.png)

#### 7. Resource Cleanup
{{% notice warning "Warning" %}}
After completion, delete the resources to avoid unnecessary costs.
{{% /notice %}}

1. **Delete RDS**: Select DB Instance -> **Delete** (Uncheck Create final snapshot).
2. **Delete DB Snapshot**: Select Snapshots -> **Delete**.
3. **Terminate EC2**: Select Instance -> **Instance state** -> **Terminate**.
4. **Release Elastic IP** (if applicable).
5. **Delete VPC**: Delete Security Groups, NAT Gateways first, then delete the VPC.

