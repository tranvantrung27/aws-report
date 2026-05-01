---
title: "Week 1 Worklog"
date: 2026-04-20
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---

### Week 1 Objectives:

* Connect and get acquainted with members of First Cloud Journey.
* Understand basic AWS services, how to use the console & CLI.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                   | Start Date | Completion Date | Reference Material                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 2   | - Get acquainted with FCJ members <br> - Read and take note of the rules and regulations at the internship unit                                                                                      | 20/04/2026 | 20/04/2026 |                                           |
| 3   | - Learn about AWS and its service categories <br>&emsp; + Compute <br>&emsp; + Storage <br>&emsp; + Networking <br>&emsp; + Database <br>&emsp; + ... <br>                                          | 21/04/2026 | 21/04/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - Create an AWS Free Tier account <br> - Learn about AWS Console & AWS CLI <br> - **Practice:** <br>&emsp; + Create an AWS account <br>&emsp; + Install and configure AWS CLI <br> &emsp; + Learn how to use AWS CLI | 22/04/2026 | 22/04/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - Learn the basics of EC2: <br>&emsp; + Instance types <br>&emsp; + AMI <br>&emsp; + EBS <br>&emsp; + ... <br> - Explore methods for remotely connecting to EC2 via SSH <br> - Learn about Elastic IP <br> | 23/04/2026 | 23/04/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **Practice:** <br>&emsp; + Create an EC2 instance <br>&emsp; + Connect via SSH <br>&emsp; + Attach an EBS volume                                                                                   | 24/04/2026 | 24/04/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 7 - Sun | - **Advanced practice:** <br>&emsp; + Recreate the EC2 instance and configure the Security Group <br>&emsp; + Connect via SSH and install a Web Server (Apache/Nginx) <br>&emsp; + Deploy a simple website and access it through the Public IP <br>&emsp; + Learn and experiment with AWS S3 (create a bucket, upload files) <br>&emsp; + Practice more AWS CLI commands | 25/04/2026 | 26/04/2026 | <https://cloudjourney.awsstudygroup.com/> |

### Week 1 Achievements:

* Understood what AWS is and gained a solid grasp of the basic service groups:
  * Compute (EC2)
  * Storage (S3, EBS)
  * Networking (VPC)
  * Database (RDS, DynamoDB)

---

* Successfully created and configured an AWS Free Tier account, ready for deploying and testing AWS services.

---

* Became familiar with the AWS Management Console:
  * Learned how to search for, access, and use AWS services
  * Performed resource management tasks through the web interface

---

* Installed and configured AWS CLI on the computer:
  * Set up the Access Key and Secret Key
  * Configured the default region
  * Verified CLI connectivity with the AWS account

---

* Used AWS CLI to perform operations such as:
  * Checking configuration information (`aws configure list`)
  * Retrieving the list of regions (`aws ec2 describe-regions`)
  * Viewing EC2 instance information
  * Creating and managing key pairs
  * Checking resource status

---

* Successfully deployed an EC2 instance:
  * Selected an AMI (Amazon Linux)
  * Configured an instance type suitable for the Free Tier
  * Set up the Security Group

---

* Successfully connected to the EC2 instance via SSH:
  * Used a key pair to access the server
  * Performed basic Linux commands

---

* Installed and tested a web server (Apache/Nginx) on EC2:
  * Successfully accessed it through the Public IP
  * Verified the server was running properly

---

* Created and attached an EBS volume to the EC2 instance:
  * Mounted it into the file system
  * Verified its data storage capability

---

* (Optional - if completed)
* Created an S3 bucket and uploaded test files to the storage service.

---

* Gained the ability to combine AWS Management Console and AWS CLI to manage AWS resources flexibly and effectively.
