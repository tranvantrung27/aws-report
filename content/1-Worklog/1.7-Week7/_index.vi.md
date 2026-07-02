---
title: "Worklog Tuần 7"
date: 2026-06-01
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Mục tiêu tuần 7:

* Kết nối, làm quen với các thành viên trong First Cloud Journey.
* Hiểu dịch vụ AWS cơ bản, cách dùng console & CLI.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Tìm hiểu AWS CLI & Cài đặt <br> - Xem tài nguyên qua CLI <br> - AWS CLI với Amazon S3 và Amazon SNS | 01/06/2026 | 01/06/2026 | [Installing the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) <br> [AWS CLI Command Reference](https://awscli.amazonaws.com/v2/documentation/api/latest/index.html) |
| 3 | - AWS CLI với IAM và VPC (VPC, Internet Gateway) <br> - Thực hành khởi tạo EC2 bằng AWS CLI | 02/06/2026 | 02/06/2026 | [AWS CLI for EC2 & VPC](https://docs.aws.amazon.com/cli/latest/userguide/cli-services-ec2.html) <br> [AWS CLI for IAM](https://docs.aws.amazon.com/cli/latest/userguide/cli-services-iam.html) |
| 4 | - Tìm hiểu AWS Organizations <br> - Tạo tài khoản phụ, thiết lập Organizational Unit (OU) và mời AWS Account tham gia | 03/06/2026 | 03/06/2026 | [AWS Organizations User Guide](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_introduction.html) <br> [Creating account in Organization](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_accounts_create.html) |
| 5 | - Cấu hình truy cập Member Account qua CLI <br> - Thiết lập Time-based access control & Customer Managed Policies <br> - Tìm hiểu IAM Identity Center Identity Store APIs | 04/06/2026 | 04/06/2026 | [AWS IAM Identity Center Guide](https://docs.aws.amazon.com/singlesignon/latest/userguide/what-is.html) <br> [Identity Store APIs](https://docs.aws.amazon.com/singlesignon/latest/IdentityStoreAPIReference/welcome.html) |
| 6 | - Tìm hiểu dịch vụ AWS Backup <br> - Chuẩn bị môi trường: Tạo S3 Bucket & Triển khai hạ tầng phục vụ Backup | 05/06/2026 | 05/06/2026 | [What is AWS Backup?](https://docs.aws.amazon.com/aws-backup/latest/devguide/whatisbackup.html) <br> [Backup S3 with AWS Backup](https://docs.aws.amazon.com/aws-backup/latest/devguide/s3-backups.html) |
| 7 - CN | - Thiết lập Backup plan & cấu hình thông báo (Notifications) <br> - Thực hành khôi phục (Test Restore) <br> - Dọn dẹp tài nguyên để tránh phát sinh chi phí | 06/06/2026 | 07/06/2026 | [Creating backup plans](https://docs.aws.amazon.com/aws-backup/latest/devguide/creating-backup-plans.html) <br> [Restoring a backup](https://docs.aws.amazon.com/aws-backup/latest/devguide/restoring-backups.html) |

### Kết quả đạt được tuần 7:

---

### Chi tiết thực hành Tuần 7:

#### Lab 1: Tương tác với tài nguyên bằng AWS CLI (Lab 11)
##### 1. Giới thiệu (Introduction)
##### 2. Chuẩn bị (Preparation)
##### 3. Cài đặt AWS CLI (Install AWS CLI)
##### 4. Xem tài nguyên qua CLI (View resource via CLI)
##### 5. AWS CLI với Amazon S3
##### 6. AWS CLI với Amazon SNS
##### 7. AWS CLI với IAM
##### 8. AWS CLI với VPC
###### 8.1 AWS CLI với VPC
###### 8.2 AWS CLI với Internet Gateway
##### 9. Khởi tạo EC2 bằng AWS CLI (Creating EC2 Using AWS CLI)
##### 10. Xử lý sự cố (Troubleshooting)
##### 11. Dọn dẹp tài nguyên (Clean up resources)

---

#### Lab 2: AWS Organizations & IAM Identity Center (Lab 12)
##### 1. Các bước chuẩn bị (Preparation steps)
###### 1.1 Tạo AWS Account trong AWS Organizations
###### 1.2 Thiết lập Organizational Unit (OU)
###### 1.3 Mời AWS Account vào AWS Organization
###### 1.4 Truy cập member account trong Organization
##### 2. AWS CLI
##### 3. Kiểm soát truy cập dựa trên thời gian (Time-based access control)
##### 4. Customer Managed Policies
##### 5. IAM Identity Center Identity Store APIs
##### 6. Dọn dẹp tài nguyên (Resource Cleanup)

---

#### Lab 3: AWS Backup (Lab 13)
##### 1. Giới thiệu (Introduction)
##### 2. Chuẩn bị (Preparation)
###### 2.1 Tạo S3 Bucket
###### 2.2 Triển khai hạ tầng (Deploy infrastructure)
##### 3. Tạo Backup plan (Create Backup plan)
##### 4. Thiết lập thông báo (Set up notifications)
##### 5. Kiểm tra khôi phục (Test Restore)
##### 6. Dọn dẹp tài nguyên (Clean up resources)

