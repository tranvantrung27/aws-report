---
title: "Week 7 Worklog"
date: 2026-06-01
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Week 7 Objectives:

* Connect and get acquainted with members of First Cloud Journey.
* Understand basic AWS services, how to use the console & CLI.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | - Learn AWS CLI & Installation <br> - View resources via CLI <br> - AWS CLI with Amazon S3 and Amazon SNS | 01/06/2026 | 01/06/2026 | [Installing the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) <br> [AWS CLI Command Reference](https://awscli.amazonaws.com/v2/documentation/api/latest/index.html) |
| 3 | - AWS CLI with IAM and VPC (VPC, Internet Gateway) <br> - Practice creating EC2 via AWS CLI | 02/06/2026 | 02/06/2026 | [AWS CLI for EC2 & VPC](https://docs.aws.amazon.com/cli/latest/userguide/cli-services-ec2.html) <br> [AWS CLI for IAM](https://docs.aws.amazon.com/cli/latest/userguide/cli-services-iam.html) |
| 4 | - Learn AWS Organizations <br> - Create member accounts, configure Organizational Unit (OU) and invite AWS Accounts | 03/06/2026 | 03/06/2026 | [AWS Organizations User Guide](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_introduction.html) <br> [Creating account in Organization](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_accounts_create.html) |
| 5 | - Configure Member Account access via CLI <br> - Configure Time-based access control & Customer Managed Policies <br> - Understand IAM Identity Center Identity Store APIs | 04/06/2026 | 04/06/2026 | [AWS IAM Identity Center Guide](https://docs.aws.amazon.com/singlesignon/latest/userguide/what-is.html) <br> [Identity Store APIs](https://docs.aws.amazon.com/singlesignon/latest/IdentityStoreAPIReference/welcome.html) |
| 6 | - Learn AWS Backup service <br> - Prepare environment: Create S3 Bucket & Deploy Backup infrastructure | 05/06/2026 | 05/06/2026 | [What is AWS Backup?](https://docs.aws.amazon.com/aws-backup/latest/devguide/whatisbackup.html) <br> [Backup S3 with AWS Backup](https://docs.aws.amazon.com/aws-backup/latest/devguide/s3-backups.html) |
| 7 - Sun | - Setup Backup plan & configure notifications <br> - Test Restore operations <br> - Clean up resources to prevent charges | 06/06/2026 | 07/06/2026 | [Creating backup plans](https://docs.aws.amazon.com/aws-backup/latest/devguide/creating-backup-plans.html) <br> [Restoring a backup](https://docs.aws.amazon.com/aws-backup/latest/devguide/restoring-backups.html) |

### Week 7 Achievements:

---

### Week 7 Practice Details:

#### Lab 1: Interacting with Resources using AWS CLI (Lab 11)
##### 1. Introduction
The Amazon Web Services Command Line Interface (AWS CLI) is an open-source tool that enables interaction with AWS services using commands in your command-line shell. Utilizing the CLI helps automate administration tasks, rapidly set up resources, and eliminate manual console clicks.

##### 2. Preparation
* An active AWS account with administrative permissions.
* Configured default profile with:
  * AWS Access Key ID and Secret Access Key.
  * Default region: `ap-southeast-1` (Singapore).
  * Output format: `json`.

##### 3. Install AWS CLI
* Downloaded and installed AWS CLI v2 on Windows via MSI installer.
* Verified installation version:
```bash
aws --version
# Output: aws-cli/2.34.46 Python/3.14.4 Windows/11 exe/AMD64
```
* Configured local credentials:
```bash
aws configure
```

##### 4. View resource via CLI
* Listed existing resources on the account:
```bash
aws s3 ls
aws ec2 describe-instances --output table --region ap-southeast-1
```

##### 5. AWS CLI with Amazon S3
* Created a globally unique S3 Bucket:
```bash
aws s3 mb s3://tranvantrung-lab11-cli-2026 --region ap-southeast-1
```
* Enabled Bucket Versioning:
```bash
aws s3api put-bucket-versioning --bucket tranvantrung-lab11-cli-2026 --versioning-configuration Status=Enabled
```
* Created a test file and uploaded it to S3:
```bash
echo "Hello from AWS CLI Lab 11" > lab11-test.txt
aws s3 cp lab11-test.txt s3://tranvantrung-lab11-cli-2026/
```
* Verified bucket configuration in S3 console:

![S3 Versioning Enabled](/images/worklog/week-7/1_s3_bucket.png)

##### 6. AWS CLI with Amazon SNS
* Created a new standard SNS Topic:
```bash
aws sns create-topic --name "lab11-sns-topic"
```
* Subscribed an email endpoint to the topic:
```bash
aws sns subscribe --topic-arn "arn:aws:sns:ap-southeast-1:938834038589:lab11-sns-topic" --protocol email --notification-endpoint "tranvantrung27@gmail.com"
```
* Verified topic creation in SNS Dashboard:

![SNS Dashboard](/images/worklog/week-7/1_sns_topic.png)

##### 7. AWS CLI with IAM
* Created Group `dev` and User `dev-1`:
```bash
aws iam create-group --group-name "dev"
aws iam create-user --user-name "dev-1"
```
* Added User to Group dev:
```bash
aws iam add-user-to-group --user-name "dev-1" --group-name "dev"
```
* Created Access Key and Secret Key for the user:
```bash
aws iam create-access-key --user-name "dev-1"
```

##### 8. AWS CLI with VPC
###### 8.1 AWS CLI with VPC
* Created a new VPC with CIDR block `10.0.0.0/16`:
```bash
aws ec2 create-vpc --cidr-block 10.0.0.0/16
```
* Created Public and Private subnets:
```bash
aws ec2 create-subnet --vpc-id vpc-018752326f096b570 --cidr-block 10.0.1.0/24
aws ec2 create-subnet --vpc-id vpc-018752326f096b570 --cidr-block 10.0.2.0/24
```
* Created a route table for the public subnet:
```bash
aws ec2 create-route-table --vpc-id vpc-018752326f096b570
```

###### 8.2 AWS CLI with Internet Gateway
* Created and attached an Internet Gateway (IGW) to the VPC:
```bash
aws ec2 create-internet-gateway
aws ec2 attach-internet-gateway --vpc-id vpc-018752326f096b570 --internet-gateway-id igw-00446df6401dd9c1b
```
* Configured route table path to Internet via IGW (0.0.0.0/0) and associated it with the public subnet:
```bash
aws ec2 create-route --route-table-id rtb-0321d43c16d3d1af1 --destination-cidr-block 0.0.0.0/0 --gateway-id igw-00446df6401dd9c1b
aws ec2 associate-route-table --subnet-id subnet-071c3cdbe2ba64c01 --route-table-id rtb-0321d43c16d3d1af1
```

##### 9. Creating EC2 Using AWS CLI
* Created a security group and configured RDP/SSH access rules:
```bash
aws ec2 create-security-group --group-name "MyLabSG" --description "Lab security group" --vpc-id vpc-018752326f096b570
aws ec2 authorize-security-group-ingress --group-id sg-0735cc3fa537bb0ee --protocol tcp --port 22 --cidr 0.0.0.0/0
```
* Generated an EC2 Key Pair:
```bash
aws ec2 create-key-pair --key-name "MyKeyPair" --query "KeyMaterial" --output text > MyKeyPair.pem
```
* Launched the EC2 instance using a Free Tier eligible instance type (`t3.micro`):
```bash
aws ec2 run-instances --image-id ami-047126e50991d067b --count 1 --instance-type t3.micro --key-name "MyKeyPair" --security-group-ids sg-0735cc3fa537bb0ee --subnet-id subnet-071c3cdbe2ba64c01 --associate-public-ip-address
```
* Verified instance running state in EC2 console:

![EC2 Instance Running](/images/worklog/week-7/1_ec2_running.png)

##### 10. Troubleshooting
* **InvalidParameterCombination:** Default `t2.micro` was blocked because of AWS Academy Starter restrictions. Fixed by using `t3.micro` which is permitted under current student policies.
* **BucketNotEmpty:** Deleting a versioned bucket failed. Fixed by listing and deleting all versions and delete markers first.

##### 11. Clean up resources
Cleaned up all resources to avoid charges:
```bash
aws ec2 terminate-instances --instance-ids i-0d613d864f17ca80a
aws ec2 delete-security-group --group-id sg-0735cc3fa537bb0ee
aws ec2 delete-subnet --subnet-id subnet-071c3cdbe2ba64c01
aws ec2 delete-subnet --subnet-id subnet-0bbcfd2ca287686cf
aws ec2 delete-route-table --route-table-id rtb-0321d43c16d3d1af1
aws ec2 detach-internet-gateway --internet-gateway-id igw-00446df6401dd9c1b --vpc-id vpc-018752326f096b570
aws ec2 delete-internet-gateway --internet-gateway-id igw-00446df6401dd9c1b
aws ec2 delete-vpc --vpc-id vpc-018752326f096b570
aws s3 rb s3://tranvantrung-lab11-cli-2026/ --force
aws sns delete-topic --topic-arn "arn:aws:sns:ap-southeast-1:938834038589:lab11-sns-topic"
aws iam delete-user --user-name "dev-1"
aws iam delete-group --group-name "dev"
```


---

#### Lab 2: AWS Organizations & IAM Identity Center (Lab 12)
##### 1. Preparation steps
###### 1.1 Create AWS Account in AWS Organizations
###### 1.2 Setting up the Organization Unit
###### 1.3 Invite AWS Account to AWS Organization
###### 1.4 Access member account in Organization
##### 2. AWS CLI
##### 3. Time-based access control
##### 4. Customer Managed Policies
##### 5. IAM Identity Center Identity Store APIs
##### 6. Resource Cleanup

---

#### Lab 3: AWS Backup (Lab 13)
##### 1. Introduction
##### 2. Preparation
###### 2.1 Create S3 Bucket
###### 2.2 Deploy infrastructure
##### 3. Create Backup plan
##### 4. Set up notifications
##### 5. Test Restore
##### 6. Clean up resources
