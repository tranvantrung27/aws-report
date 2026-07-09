---
title: "Worklog Tuần 11"
date: 2026-06-29
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

### Mục tiêu tuần 11:

* Thực hành triển khai CI/CD pipeline với AWS CodePipeline, CodeCommit, CodeBuild và CodeDeploy.
* Tìm hiểu và cấu hình AWS File Storage Gateway kết nối môi trường On-premises.
* Cấu hình bảo mật ứng dụng với AWS Web Application Firewall (WAF).
* Quản lý tài nguyên hiệu quả sử dụng Tags và Resource Groups.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - **Thực hành Deploy applications to EC2 with AWS CodePipeline (Phần 1)** <br>&emsp; + Chuẩn bị môi trường (S3, Git, IAM roles) <br>&emsp; + Khởi tạo EC2 và cài đặt CodeDeploy Agent | 29/06/2026 | 29/06/2026 | [AWS CodeDeploy Guide](https://docs.aws.amazon.com/codedeploy/latest/userguide/welcome.html) |
| 3   | - **Thực hành Deploy applications to EC2 with AWS CodePipeline (Phần 2)** <br>&emsp; + Thiết lập AWS CodeCommit, CodeBuild, CodeDeploy <br>&emsp; + Cấu hình AWS CodePipeline để tự động hóa CI/CD | 30/06/2026 | 30/06/2026 | [AWS CodePipeline Guide](https://docs.aws.amazon.com/codepipeline/latest/userguide/welcome.html) |
| 4   | - **Thực hành Using File Storage Gateway** <br>&emsp; + Tạo S3 Bucket, EC2 cho Storage Gateway <br>&emsp; + Khởi tạo Storage Gateway và File Shares <br>&emsp; + Mount File shares lên máy On-premises | 01/07/2026 | 01/07/2026 | [AWS Storage Gateway](https://docs.aws.amazon.com/storagegateway/latest/userguide/WhatIsStorageGateway.html) |
| 5   | - **Thực hành AWS Web Application Firewall (Phần 1)** <br>&emsp; + Triển khai sample Web App <br>&emsp; + Sử dụng AWS WAF, thiết lập Web ACLs và Managed rules | 02/07/2026 | 02/07/2026 | [AWS WAF Developer Guide](https://docs.aws.amazon.com/waf/latest/developerguide/waf-chapter.html) |
| 6   | - **Thực hành AWS Web Application Firewall (Phần 2)** <br>&emsp; + Tạo Custom rules chặn IP/Quốc gia <br>&emsp; + Kích hoạt Logging và kiểm thử (Testing) | 03/07/2026 | 03/07/2026 | [AWS WAF Managed Rules](https://docs.aws.amazon.com/waf/latest/developerguide/aws-managed-rule-groups.html) |
| 7 - CN | - **Thực hành Manage Resources Using Tags and Resource Groups** <br>&emsp; + Sử dụng Tags trên Console và CLI <br>&emsp; + Tạo và quản lý Resource Group | 04/07/2026 | 05/07/2026 | [Tagging AWS resources](https://docs.aws.amazon.com/tag-editor/latest/userguide/tagging.html) <br> [AWS Resource Groups](https://docs.aws.amazon.com/ARG/latest/userguide/welcome.html) |

### Kết quả đạt được tuần 11:

* Nắm vững quy trình CI/CD trên AWS, biết cách tích hợp các dịch vụ CodeCommit, CodeBuild, CodeDeploy và CodePipeline để tự động hóa việc triển khai ứng dụng.
* Hiểu và cấu hình thành công AWS File Storage Gateway, kết nối lưu trữ từ On-premises lên AWS S3.
* Triển khai thành công AWS WAF, bảo vệ ứng dụng web khỏi các cuộc tấn công thông qua Web ACLs và Custom rules.
* Biết cách tổ chức và quản lý tài nguyên AWS hiệu quả sử dụng Tags và Resource Groups.
* Hoàn thành tốt 4 bài thực hành theo hướng dẫn.

---

### Chi tiết thực hành Tuần 11:

#### Deploy applications to EC2 with AWS CodePipeline

##### 1. Giới thiệu
Thực hành xây dựng hệ thống CI/CD hoàn chỉnh sử dụng các dịch vụ AWS Developer Tools để triển khai mã nguồn lên máy chủ EC2 một cách tự động. Thay vì sử dụng CodeCommit (đã ngừng hỗ trợ cho tài khoản mới), bài thực hành này tích hợp **GitHub** làm Source stage kết hợp với **CodeBuild**, **CodeDeploy** và quản lý tự động bởi **CodePipeline**.

##### 2. Các bước triển khai chi tiết
###### **Bước 1: Chuẩn bị tài nguyên (Preparation)**
* **Tạo S3 Bucket để chứa Artifacts:**
  * Tạo S3 bucket và cấu hình Bucket Policy cho phép truy cập.
  ![S3 Bucket Policy](/images/worklog/week-11/1_s3_bucket_policy.png)
  ![S3 Bucket List](/images/worklog/week-11/2_s3_bucket_list.png)
* **Tạo IAM Service Role cho CodeDeploy:**
  * Tạo IAM Role đặt tên là `CodeDeployServiceRoleEC2` với Trust Policy cho phép dịch vụ CodeDeploy hoạt động và đính kèm policy `AWSCodeDeployRole`.
  ![CodeDeploy Service Role](/images/worklog/week-11/4_codedeploy_service_role.png)
* **Tạo IAM Instance Profile cho EC2:**
  * Tạo IAM Role đặt tên là `EC2InstanceRoleForCodeDeployV2` chọn Use case là **EC2** (để tự động tạo Instance Profile tương thích).
  * Đính kèm các permissions policies: `AmazonS3ReadOnlyAccess` và `AmazonSSMManagedInstanceCore` để cho phép EC2 đọc file build từ S3 và kết nối an toàn qua Systems Manager.
  ![EC2 Instance Role V2](/images/worklog/week-11/6_create_role_v2_success.png)

###### **Bước 2: Cài đặt CodeDeploy Agent & Khởi tạo EC2**
* Khởi chạy một EC2 instance với hệ điều hành Amazon Linux 2023 (`ap-southeast-1` Singapore) tên là `CodeDeployEC2`.
* Chọn Instance Profile vừa tạo: `EC2InstanceRoleForCodeDeployV2`.
* Trong phần **User Data**, nhập script cài đặt tự động CodeDeploy Agent:
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
* Khởi chạy EC2 thành công:
  ![Launch EC2 Success](/images/worklog/week-11/7_launch_ec2_success.png)

###### **Bước 3: Thiết lập Source Repository (GitHub)**
* Thay vì CodeCommit, khởi tạo một GitHub repository mới tên là `aws-fcj-pipeline` chế độ Public.
  ![GitHub Repo Created](/images/worklog/week-11/8_github_repo_success.png)
* Khởi tạo dự án local, thêm các file cấu hình và mã nguồn:
  * `index.html`: Trang Web hiển thị giao diện mẫu.
  * `appspec.yml`: Chỉ định các hành động Deploy lên Apache Web Server trên EC2.
  * `buildspec.yml`: Chỉ định quy trình Build & Artifacts của CodeBuild.
  * `scripts/install_dependencies`: File script shell cài đặt gói dịch vụ `httpd` (Apache) trước khi cài ứng dụng.
* Đẩy mã nguồn từ máy local lên GitHub thành công.

###### **Bước 4: Cấu hình AWS CodeBuild (Hoàn thành)**
* **Kết nối GitHub với CodeBuild:**
  * Truy cập dịch vụ CodeBuild tại region `ap-southeast-1` (Singapore).
  * Tạo Build project mới, chọn Source provider là **GitHub**.
  * AWS yêu cầu xác thực GitHub OAuth - đăng nhập tài khoản GitHub `tranvantrung27` và cấp quyền (Authorize) cho AWS CodeBuild truy cập repository.
  * Xác thực 2 lớp (2FA) trên GitHub qua thiết bị di động thành công.
  * Kết nối GitHub OAuth thành công, CodeBuild nhận diện được tài khoản và repository.

* **Tạo Build Project `AWS-FCJ-APP` thành công:**
  * **Source provider:** GitHub
  * **Repository:** `https://github.com/tranvantrung27/aws-fcj-pipeline.git` (sử dụng GitHub repository vừa tạo ở bước 3)
  * **Environment:**
    * Operating System: Amazon Linux
    * Runtime: Standard
    * Image: `aws/codebuild/amazonlinux-x86_64-standard:5.0` (Standard Image 5.0)
    * Service role: Sử dụng role đã được tạo tự động `codebuild-AWS-FCJ-APP-service-role`
  * **Buildspec:** Sử dụng file `buildspec.yml` có sẵn trong mã nguồn
  * **Artifacts:**
    * Type: Amazon S3
    * Bucket name: `aws-cicd-ec2-trantrung04`
  * **Logs:**
    * CloudWatch Logs: Enabled
    * Group name: `aws-cicd-ec2-group`
    * Stream name: `aws-cicd-ec2-stream`
  * Project tạo thành công, Configuration hiển thị đầy đủ thông tin trên Console.
  <!-- ![CodeBuild Project Created](/images/worklog/week-11/9_codebuild_project_created.png) -->

###### **Bước 5: Cấu hình AWS CodeDeploy (Đang chờ)**
* Tạo Application tên `AWS-FCJ-APP` với Compute platform: **EC2/On-premises**.
* Tạo Deployment Group tên `AWS-FCJ-DG`:
  * Service Role: Role có policy `AWSCodeDeployRole` (đã tạo ở Bước 1: `CodeDeployServiceRoleEC2`).
  * EC2 Instance: Chọn theo tag Key=`Name`, Value=`CodeDeployEC2`.
  * Deployment Configuration: `CodeDeployDefault.OneAtATime`.
  * Load Balancer: Disabled.
* **Lưu ý:** Tại thời điểm thực hành, CodeDeploy Console bị chuyển hướng sang trang "Complete your account setup" do tài khoản AWS đang trong quá trình xác minh. Cần chờ xác minh hoàn tất để tiếp tục.

###### **Bước 6: Tự động hóa bằng AWS CodePipeline (Đang chờ)**
* Tạo Pipeline kết nối: Source (GitHub `tranvantrung27/aws-fcj-pipeline`) -> Build (CodeBuild `AWS-FCJ-APP`) -> Deploy (CodeDeploy `AWS-FCJ-APP` / `AWS-FCJ-DG`).
* Kiểm thử quá trình tự động triển khai khi có commit mới.
* CodePipeline Console đã truy cập thành công (Pipelines page hiển thị trống, chưa có pipeline nào).


#### Using File Storage Gateway
##### 1. Giới thiệu
Thực hành tạo File Storage Gateway để kết nối dữ liệu từ môi trường lưu trữ cục bộ (On-premises) lên Amazon S3 thông qua giao thức truyền thống như NFS/SMB.

##### 2. Các bước triển khai
* **Bước 1: Chuẩn bị tài nguyên**
  * Tạo một S3 bucket dùng làm nơi lưu trữ dữ liệu backend.
  * Khởi tạo máy chủ EC2 đóng vai trò là Storage Gateway appliance (mô phỏng máy chủ tại môi trường On-premises).
* **Bước 2: Khởi tạo Storage Gateway**
  * Truy cập AWS Storage Gateway Console, tạo gateway mới thuộc loại **Amazon S3 File Gateway**.
  * Kết nối và kích hoạt gateway thông qua địa chỉ IP của máy chủ EC2.
* **Bước 3: Tạo File Share**
  * Cấu hình File Share liên kết Gateway với S3 bucket đã tạo. 
  * Lấy lệnh mount (NFS/SMB) từ AWS Console.
* **Bước 4: Mount File Share lên máy On-premises**
  * Đăng nhập vào máy client.
  * Thực thi lệnh mount thư mục được chia sẻ vào hệ thống tệp cục bộ và kiểm tra quá trình đồng bộ file tự động lên S3.

#### AWS Web Application Firewall (WAF)
##### 1. Giới thiệu
Sử dụng AWS WAF để bảo vệ ứng dụng web khỏi các cuộc tấn công mạng phổ biến (như SQL Injection, Cross-Site Scripting) và quản lý lưu lượng truy cập dựa trên các quy tắc tùy chỉnh (IP, vị trí địa lý).

##### 2. Các bước triển khai
* **Bước 1: Triển khai ứng dụng Web mẫu**
  * Khởi tạo một Web Application mẫu thông qua CloudFormation hoặc triển khai thủ công để làm mục tiêu gắn WAF.
* **Bước 2: Thiết lập Web ACLs với Managed Rules**
  * Truy cập AWS WAF, tạo một Web ACL (Access Control List).
  * Thêm các quy tắc quản lý sẵn của AWS (**Managed Rules**) như *AWSManagedRulesCommonRuleSet* để bảo vệ khỏi các rủi ro thông dụng.
* **Bước 3: Tạo Custom Rules**
  * Tạo các **Custom Rules** cơ bản và nâng cao để chặn (Block) truy cập từ một địa chỉ IP hoặc quốc gia cụ thể.
* **Bước 4: Kiểm thử và Logging**
  * Truy cập thử vào trang web bằng IP bị cấm để nhận lỗi `403 Forbidden`.
  * Kích hoạt Logging để giám sát các requests bị chặn hoặc cho phép.

#### Manage Resources Using Tags and Resource Groups
##### 1. Giới thiệu
Thực hành gắn thẻ (Tags) cho tài nguyên để dễ dàng phân loại, quản lý chi phí và tạo Resource Groups nhằm nhóm các tài nguyên lại với nhau cho mục đích vận hành, giám sát.

##### 2. Các bước triển khai
* **Bước 1: Sử dụng Tags trên AWS Console**
  * Khởi tạo một tài nguyên (ví dụ: EC2 instance) và gắn các tag như `Environment: Dev`, `Project: Workshop`.
  * Quản lý (thêm/xóa) tags cho nhiều tài nguyên cùng lúc.
* **Bước 2: Filter tài nguyên theo Tag**
  * Sử dụng công cụ Tag Editor để tìm kiếm tất cả các tài nguyên có chung một Tag cụ thể.
* **Bước 3: Sử dụng CLI để quản lý Tag**
  * Dùng lệnh AWS CLI (vd: `aws ec2 create-tags`) để gắn thẻ hoặc truy vấn tài nguyên trực tiếp từ terminal.
* **Bước 4: Tạo Resource Group**
  * Tạo một Resource Group (Tag-based) để tự động nhóm tất cả các tài nguyên thỏa mãn điều kiện Tag, giúp dễ dàng quản lý tập trung.
