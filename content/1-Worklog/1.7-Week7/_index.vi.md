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
Amazon Web Services Command Line Interface (AWS CLI) là một công cụ mã nguồn mở giúp tương tác với các dịch vụ của AWS thông qua các dòng lệnh trong màn hình terminal. Việc sử dụng CLI giúp tự động hóa các tác vụ quản trị, thiết lập tài nguyên nhanh chóng và loại bỏ các thao tác click thủ công tốn thời gian trên AWS Management Console.

##### 2. Chuẩn bị (Preparation)
* Tài khoản AWS đang hoạt động với quyền quản trị.
* Thiết lập cấu hình profile mặc định với các tham số:
  * AWS Access Key ID và Secret Access Key.
  * Default region: `ap-southeast-1` (Singapore).
  * Output format: `json`.

##### 3. Cài đặt AWS CLI (Install AWS CLI)
* Tải xuống và cài đặt AWS CLI v2 trên Windows bằng gói cài đặt MSI.
* Kiểm tra phiên bản cài đặt thành công:
```bash
aws --version
# Kết quả: aws-cli/2.34.46 Python/3.14.4 Windows/11 exe/AMD64
```
* Cấu hình thông tin xác thực:
```bash
aws configure
```

##### 4. Xem tài nguyên qua CLI (View resource via CLI)
* Liệt kê các tài nguyên hiện có trên tài khoản:
```bash
aws s3 ls
aws ec2 describe-instances --output table --region ap-southeast-1
```

##### 5. AWS CLI với Amazon S3
* Khởi tạo S3 Bucket có tên duy nhất toàn cầu:
```bash
aws s3 mb s3://tranvantrung-lab11-cli-2026 --region ap-southeast-1
```
* Bật tính năng lưu trữ phiên bản (Versioning) cho Bucket:
```bash
aws s3api put-bucket-versioning --bucket tranvantrung-lab11-cli-2026 --versioning-configuration Status=Enabled
```
* Tạo tệp tin thử nghiệm và tải lên S3:
```bash
echo "Hello from AWS CLI Lab 11" > lab11-test.txt
aws s3 cp lab11-test.txt s3://tranvantrung-lab11-cli-2026/
```
* Xác minh trạng thái cấu hình Bucket trên Console:

![S3 Versioning Enabled](/images/worklog/week-7/1_s3_bucket.png)

##### 6. AWS CLI với Amazon SNS
* Khởi tạo một SNS Topic mới:
```bash
aws sns create-topic --name "lab11-sns-topic"
```
* Đăng ký nhận thông báo qua email:
```bash
aws sns subscribe --topic-arn "arn:aws:sns:ap-southeast-1:938834038589:lab11-sns-topic" --protocol email --notification-endpoint "tranvantrung27@gmail.com"
```
* Xác minh SNS Topic hiển thị trên Console:

![SNS Dashboard](/images/worklog/week-7/1_sns_topic.png)

##### 7. AWS CLI với IAM
* Tạo Group `dev` và User `dev-1`:
```bash
aws iam create-group --group-name "dev"
aws iam create-user --user-name "dev-1"
```
* Thêm User vào Group dev:
```bash
aws iam add-user-to-group --user-name "dev-1" --group-name "dev"
```
* Khởi tạo Access Key và Secret Access Key cho user:
```bash
aws iam create-access-key --user-name "dev-1"
```

##### 8. AWS CLI với VPC
###### 8.1 AWS CLI với VPC
* Khởi tạo VPC mới với dải mạng CIDR `10.0.0.0/16`:
```bash
aws ec2 create-vpc --cidr-block 10.0.0.0/16
```
* Khởi tạo Public Subnet và Private Subnet:
```bash
aws ec2 create-subnet --vpc-id vpc-018752326f096b570 --cidr-block 10.0.1.0/24
aws ec2 create-subnet --vpc-id vpc-018752326f096b570 --cidr-block 10.0.2.0/24
```
* Tạo Route Table cho Public Subnet:
```bash
aws ec2 create-route-table --vpc-id vpc-018752326f096b570
```

###### 8.2 AWS CLI với Internet Gateway
* Khởi tạo Internet Gateway (IGW) và liên kết vào VPC:
```bash
aws ec2 create-internet-gateway
aws ec2 attach-internet-gateway --vpc-id vpc-018752326f096b570 --internet-gateway-id igw-00446df6401dd9c1b
```
* Cấu hình Route chuyển tiếp dữ liệu ra internet qua IGW (0.0.0.0/0) và liên kết subnet công cộng:
```bash
aws ec2 create-route --route-table-id rtb-0321d43c16d3d1af1 --destination-cidr-block 0.0.0.0/0 --gateway-id igw-00446df6401dd9c1b
aws ec2 associate-route-table --subnet-id subnet-071c3cdbe2ba64c01 --route-table-id rtb-0321d43c16d3d1af1
```

##### 9. Khởi tạo EC2 bằng AWS CLI (Creating EC2 Using AWS CLI)
* Tạo Security Group cho máy ảo EC2 và mở cổng truy cập SSH (Port 22):
```bash
aws ec2 create-security-group --group-name "MyLabSG" --description "Lab security group" --vpc-id vpc-018752326f096b570
aws ec2 authorize-security-group-ingress --group-id sg-0735cc3fa537bb0ee --protocol tcp --port 22 --cidr 0.0.0.0/0
```
* Tạo cặp Key Pair để truy cập SSH:
```bash
aws ec2 create-key-pair --key-name "MyKeyPair" --query "KeyMaterial" --output text > MyKeyPair.pem
```
* Khởi chạy instance EC2 (Loại máy ảo `t3.micro` thuộc danh mục Free Tier được cấu hình trên tài khoản):
```bash
aws ec2 run-instances --image-id ami-047126e50991d067b --count 1 --instance-type t3.micro --key-name "MyKeyPair" --security-group-ids sg-0735cc3fa537bb0ee --subnet-id subnet-071c3cdbe2ba64c01 --associate-public-ip-address
```
* Kiểm tra máy chủ hiển thị trạng thái `Running` trên Console:

![EC2 Instance Running](/images/worklog/week-7/1_ec2_running.png)

##### 10. Xử lý sự cố (Troubleshooting)
* **Lỗi InvalidParameterCombination:** Khi khởi tạo máy ảo, loại instance `t2.micro` mặc định bị chặn do chính sách quản lý học tập (AWS Academy chỉ cho phép các loại máy ảo thuộc Free Tier cụ thể như `t3.micro`).
  * *Khắc phục:* Thay đổi tham số `--instance-type` thành `t3.micro` để hệ thống khởi chạy thành công.
* **Lỗi BucketNotEmpty:** Khi thực hiện xóa S3 Bucket đã bật tính năng Versioning, lệnh xóa bucket thông thường sẽ lỗi vì các phiên bản ẩn vẫn còn.
  * *Khắc phục:* Sử dụng `list-object-versions` kết hợp vòng lặp xóa sạch toàn bộ `Versions` và `DeleteMarkers` trước khi xóa Bucket.

##### 11. Dọn dẹp tài nguyên (Clean up resources)
Để tránh phát sinh chi phí sau bài lab, toàn bộ tài nguyên đã được xóa sạch theo trình tự ngược lại:
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
##### 1. Các bước chuẩn bị (Preparation steps)
###### 1.1 Tạo AWS Account trong AWS Organizations
* Truy cập **AWS Organizations** console thông qua tài khoản chính (Management account).
* Chọn **Add an AWS account** > **Create an AWS account**.
* Đặt tên cho tài khoản thành viên (member account) ví dụ: `production-account` và nhập địa chỉ Email của chủ sở hữu (AWS hỗ trợ sử dụng định dạng `email+alias@domain.com` để quản lý nhiều tài khoản phụ bằng cùng một hòm thư chính).
* Chỉ định IAM role mặc định có tên `OrganizationAccountAccessRole` để phục vụ cho việc Switch Role từ tài khoản Management.

###### 1.2 Thiết lập Organizational Unit (OU)
* Trong giao diện AWS Organizations, lựa chọn thư mục gốc **Root**.
* Nhấn **Actions** và chọn **Create new** dưới mục Organizational Unit.
* Tạo các Organizational Units đại diện cho cấu trúc doanh nghiệp:
  * **Security** (Nhóm bảo mật)
  * **Shared Services** (Các dịch vụ dùng chung)
  * **Logging** (Tập trung log)
  * **Application** (Chứa các tài khoản chạy ứng dụng)
* Chọn các tài khoản phụ tương ứng và nhấp **Move** để chuyển chúng vào đúng các nhóm OU này nhằm phân tách chức năng và áp dụng chính sách quản lý SCPs sau này.

###### 1.3 Mời AWS Account vào AWS Organization
* Từ tài khoản quản lý, nhấn nút **Invite AWS account** để gửi thư mời tới một tài khoản độc lập đã tồn tại trước đó bằng cách điền AWS Account ID hoặc Email của tài khoản đó.
* Đăng nhập vào tài khoản được mời, điều hướng đến AWS Organizations console và nhấn **Accept Invitation**.

###### 1.4 Truy cập member account trong Organization
* Trên tài khoản Management, chọn **Switch role** ở góc phải màn hình.
* Nhập **Account ID** của member account và Role Name là `OrganizationAccountAccessRole` để truy cập trực tiếp vào member account mà không cần nhập thông tin đăng nhập riêng.

##### 2. AWS CLI
* Cấu hình AWS CLI tích hợp Single Sign-On (SSO) sử dụng IAM Identity Center:
```bash
aws configure sso
```
* CLI tự động tạo cổng xác thực OIDC trên trình duyệt, cho phép người dùng đăng nhập bằng tài khoản SSO và sinh ra thông tin đăng nhập tạm thời, loại bỏ nguy cơ lộ lọt AWS Access Key dài hạn.

##### 3. Kiểm soát truy cập dựa trên thời gian (Time-based access control)
* Thiết lập cơ chế kiểm soát truy cập tạm thời bằng cách đính kèm khóa điều kiện thời gian thực `aws:CurrentTime` vào Permission Set trong IAM Identity Center.
* Sử dụng chính sách Inline Policy từ chối/cho phép thực hiện tác vụ trong một khung giờ cố định sử dụng toán tử định dạng ISO 8601 (UTC).
* Ví dụ chính sách chỉ cho phép hủy máy chủ EC2 trong khoảng thời gian quy định (từ ngày 25/01/2026 đến 27/01/2026):
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "TimeBasedEC2Access",
      "Effect": "Deny",
      "Action": "ec2:TerminateInstances",
      "Resource": "*",
      "Condition": {
        "DateGreaterThan": { "aws:CurrentTime": "2026-01-25T00:00:00Z" },
        "DateLessThan": { "aws:CurrentTime": "2026-01-27T23:59:59Z" }
      }
    }
  ]
}
```

##### 4. Customer Managed Policies
* permission set trong IAM Identity Center cho phép liên kết trực tiếp với các chính sách IAM cục bộ (**Customer Managed Policies**) nằm trên tài khoản thành viên.
* **Quy tắc:** Tên chính sách IAM tại tài khoản thành viên phải khớp chính xác từng chữ cái (case-sensitive) với cấu hình khai báo ở Permission Set. Điều này giúp đồng nhất về mặt quản trị cấu trúc nhưng vẫn cho phép nội dung chính sách tùy biến theo tài nguyên đặc thù của từng tài khoản.

##### 5. IAM Identity Center Identity Store APIs
* Quản trị tự động bằng lập trình thay vì thao tác Console bằng cách sử dụng kịch bản Python tương tác với AWS Identity Store APIs (thư viện Boto3).
* Lấy **Identity store ID** (dạng `d-xxxxxxxxxx`) từ phần cấu hình IAM Identity Center và thực hiện các câu lệnh quản trị:
```bash
# Xem các thao tác được hỗ trợ
python identitystore_operations.py -h

# Tạo một group mới đại diện cho nhóm khoa học dữ liệu
python identitystore_operations.py create_group --identitystoreid d-123456a7890 --groupname AWS_Data_Science --description "Data Science group"

# Tạo một User mới và thêm trực tiếp vào nhóm vừa tạo
python identitystore_operations.py create_user --identitystoreid d-123456a7890 --username johndoe --givenname John --familyname Doe --groupname AWS_Data_Science
```

##### 6. Dọn dẹp tài nguyên (Resource Cleanup)
* Đảm bảo xóa sạch cấu hình thử nghiệm bằng cách truy cập **IAM Identity Center** > **Settings** > tab **Management** và tiến hành chọn **Delete IAM Identity Center configuration** bằng cách nhập ID để xác nhận xóa.
* Xóa toàn bộ các CloudFormation stacks liên quan tại các tài khoản thành viên.

* Minh chứng giao diện quản lý IAM Identity Center:

![IAM Identity Center Dashboard](/images/worklog/week-7/2_iam_identity_center.png)


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

