---
title: "Worklog Tuần 9"
date: 2026-06-15
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

### Mục tiêu tuần 9:

* Tìm hiểu và thực hành triển khai ứng dụng trên **Amazon ECS (Elastic Container Service)** sử dụng Fargate, ALB, và Cloud Map.
* Thực hành thiết lập quy trình tự động hóa **CI/CD** cho ứng dụng bằng **GitLab CI/CD**, **GitHub Actions**, và **AWS CodeBuild**.
* Cấu hình giám sát container bằng **Amazon CloudWatch Container Insights** và định tuyến log thông qua **AWS Firelens**.
* Sử dụng **AWS Security Hub** để đánh giá điểm an toàn hệ thống (Security Score) theo các tiêu chuẩn bảo mật của AWS.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Tìm hiểu lý thuyết về Amazon ECS, Fargate, Task Definitions, và cách hoạt động của ECS Cluster | 15/06/2026 | 15/06/2026 | [Amazon ECS Developer Guide](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/Welcome.html) |
| 3 | - **Lab 16 (Phần 1):** Cấu hình hạ tầng VPC, gán Subnet, NAT Gateway, Security Group, và chuẩn bị container images đẩy lên ECR/Docker Hub | 16/06/2026 | 16/06/2026 | [AWS ECR User Guide](https://docs.aws.amazon.com/AmazonECR/latest/userguide/what-is-ecr.html) |
| 4 | - **Lab 16 (Phần 2):** Khởi tạo ECS Cluster, Task Definition cho Frontend/Backend, cấu hình Target Group, ALB và chạy ECS Service (Blue/Green Deployment cho Backend & Rolling Update cho Frontend) | 17/06/2026 | 17/06/2026 | [AWS ECS Blue/Green Deployments](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/deployment-type-blue-green.html) |
| 5 | - **Lab 17 (Phần 1):** Tìm hiểu và thực hành tích hợp CI/CD tự động bằng GitLab Runner (setup IAM role, variables, chạy pipeline) và GitHub Actions | 18/06/2026 | 18/06/2026 | [GitLab CI/CD Docs](https://docs.gitlab.com/ee/ci/) <br> [GitHub Actions Docs](https://docs.github.com/en/actions) |
| 6 | - **Lab 17 (Phần 2):** Cấu hình AWS CodeBuild cho Frontend/Backend; cấu hình giám sát Container Insights (CloudWatch) và log routing bằng AWS Firelens gửi về S3 | 19/06/2026 | 19/06/2026 | [AWS CodeBuild User Guide](https://docs.aws.amazon.com/codebuild/latest/userguide/welcome.html) <br> [AWS Firelens Guide](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/using_firelens.html) |
| 7 - CN | - **Lab 18:** Tìm hiểu AWS Security Hub, kích hoạt các tiêu chuẩn bảo mật (Security Standards), đánh giá điểm số bảo mật (Security Score) và dọn dẹp tài nguyên để tránh phát sinh chi phí | 20/06/2026 | 21/06/2026 | [AWS Security Hub User Guide](https://docs.aws.amazon.com/securityhub/latest/userguide/what-is-securityhub.html) |

### Kết quả đạt được tuần 9:

* **Amazon ECS (Fargate):** Triển khai thành công kiến trúc containerized trên ECS với hai dịch vụ độc lập Frontend và Backend mà không cần quản lý máy chủ EC2.
* **Cân bằng tải & Định tuyến:** Cấu hình thành công Application Load Balancer (ALB) hỗ trợ định tuyến dịch vụ và Blue/Green Deployment (qua AWS CodeDeploy) cho Backend, cùng Rolling Update cho Frontend.
* **CI/CD đa nền tảng:** Thiết lập hệ thống tự động hóa CI/CD thông qua GitLab CI (sử dụng GitLab Runner tự cài đặt), GitHub Actions (workflow tự động), và AWS CodeBuild.
* **Giám sát & Quản lý log:** Triển khai giải pháp giám sát tập trung CloudWatch Container Insights và giải pháp Logs Router thông qua Firelens (sử dụng Fluent Bit) để lọc và đẩy log trực tiếp về Amazon S3.
* **AWS Security Hub:** Đánh giá an ninh toàn diện và nắm được điểm số bảo mật (Security Score) dựa trên các tiêu chuẩn AWS Foundational Security Best Practices và CIS AWS Foundations Benchmark.

---

### Chi tiết thực hành Tuần 9:

#### Lab 1: Triển khai ứng dụng lên Amazon ECS (Lab 16)

##### 1. Giới thiệu (Introduction)
Trong bài thực hành này, chúng ta sẽ tiến hành container hóa ứng dụng và triển khai lên **Amazon ECS (Elastic Container Service)** sử dụng **AWS Fargate** (Serverless compute engine cho containers). Kiến trúc ứng dụng gồm 2 phần: Frontend và Backend giao tiếp với nhau. Chúng ta cũng cấu hình định tuyến thông qua **Application Load Balancer** với mô hình triển khai **Blue/Green (Backend)** và **Rolling Update (Frontend)**.

##### 2. Chuẩn bị (Preparation)

###### 2.1 Cấu hình hạ tầng (Infrastructure Configuration)
Thiết lập VPC tùy chỉnh có tên là `ECS-VPC` với dải CIDR `10.0.0.0/16`. Tạo đầy đủ các tài nguyên mạng bao gồm Internet Gateway, NAT Gateway để đảm bảo định tuyến cho các container trong vùng Private Subnet ra ngoài Internet tải packages hoặc kéo images.

###### 2.2 Tạo CodeDeploy Role
Tạo một IAM Role dành riêng cho AWS CodeDeploy với policy `AWSCodeDeployRoleForECS` để cấp quyền cho CodeDeploy thực hiện điều phối lưu lượng (traffic shifting) giữa Target Group Blue và Target Group Green trong quá trình cập nhật phiên bản ứng dụng.

###### 2.3 Thêm Subnet
Phân chia mạng VPC thành các Subnet Public và Private ở các Availability Zone khác nhau để tăng tính sẵn sàng (High Availability).
* **Public Subnet 1 & 2** (dành cho Application Load Balancer)
* **Private Subnet 1 & 2** (dành cho ECS Tasks chạy Backend/Frontend)

###### 2.4 Cấu hình NAT Gateway
Khởi tạo NAT Gateway đặt tại vùng mạng Public Subnet, gán Elastic IP tĩnh để các ECS Tasks chạy trong vùng Private Subnets có thể kết nối ra ngoài Internet một cách an toàn mà không bị lộ địa chỉ IP nội bộ.

###### 2.5 Cấu hình Route Table
Cấu hình bảng định tuyến:
* Bảng định tuyến Public trỏ dải `0.0.0.0/0` về Internet Gateway (IGW).
* Bảng định tuyến Private trỏ dải `0.0.0.0/0` về NAT Gateway đã cấu hình.

###### 2.6 Cấu hình Security Group
Thiết lập các nhóm bảo mật (Security Groups):
* `ALB-SG`: Cho phép lưu lượng HTTP (Port 80) truy cập từ mọi nguồn (`0.0.0.0/0`).
* `ECS-Frontend-SG`: Chỉ nhận traffic từ `ALB-SG` trên cổng port tương ứng.
* `ECS-Backend-SG`: Chỉ nhận traffic từ `ECS-Frontend-SG` hoặc `ALB-SG` trên cổng port API tương ứng.

##### 3. Chuẩn bị triển khai (Prepare for Deployment)

###### 3.1. Đẩy Image lên Amazon ECR
Đăng nhập vào Amazon Elastic Container Registry (ECR), tạo các repositories cho frontend và backend, sau đó dùng các lệnh CLI để build, tag và push Docker Image từ local lên ECR:
```bash
# Đăng nhập ECR
aws ecr get-login-password --region ap-southeast-1 | docker login --username AWS --password-stdin <aws_account_id>.dkr.ecr.ap-southeast-1.amazonaws.com

# Build và push Image
docker build -t ecs-backend .
docker tag ecs-backend:latest <aws_account_id>.dkr.ecr.ap-southeast-1.amazonaws.com/ecs-backend:latest
docker push <aws_account_id>.dkr.ecr.ap-southeast-1.amazonaws.com/ecs-backend:latest
```

###### 3.2. Đẩy Image lên Dockerhub
Đăng nhập vào tài khoản Docker Hub của cá nhân, tag hình ảnh và đẩy lên để làm phương án backup/multiregistry deployment:
```bash
docker login -u <username>
docker tag ecs-frontend:latest <username>/ecs-frontend:latest
docker push <username>/ecs-frontend:latest
```

##### 4. Đăng ký Namespace trong AWS Cloud Map
Cấu hình **AWS Cloud Map** để kích hoạt tính năng phát hiện dịch vụ (Service Discovery). Đăng ký một namespace riêng tư (ví dụ: `ecs-app.local`) gắn vào VPC để các ECS Tasks có thể tự động tìm thấy nhau qua tên miền nội bộ thay vì sử dụng IP động.

##### 5. Khởi tạo ECS Cluster
Truy cập Amazon ECS Console, thực hiện tạo một ECS Cluster mới với tên là `ECS-App-Cluster`. Sử dụng tùy chọn hạ tầng là **Fargate** để tận dụng mô hình Serverless giúp giảm tải việc quản lý máy chủ EC2.

##### 6. Thiết lập ECS Task Definition
Task Definition là bản thiết kế (blueprint) mô tả container sẽ chạy như thế nào (CPU, RAM, Image link, Environment variables, Log configuration).

###### 6.1. Backend Task Definition
* **Name**: `backend-task`
* **CPU / Memory**: `0.25 vCPU / 0.5 GB RAM`
* **Container Image**: Đường dẫn ECR Image backend đã push ở bước 3.
* **Port Mapping**: Cấu hình cổng `5000` cho ứng dụng Backend.
* **Log Configuration**: Bật awslogs để gửi log trực tiếp về CloudWatch Logs.

###### 6.2. Frontend Task Definition
* **Name**: `frontend-task`
* **CPU / Memory**: `0.25 vCPU / 0.5 GB RAM`
* **Container Image**: Đường dẫn ECR hoặc Docker Hub Image frontend.
* **Port Mapping**: Cấu hình cổng `80` hoặc `3000` tùy theo thiết kế ứng dụng.
* **Environment Variable**: Cấu hình biến môi trường trỏ API endpoint về địa chỉ của Backend (thông qua DNS Cloud Map hoặc Load Balancer).

##### 7. Cấu hình Application Load Balancer

###### 7.1. Tạo Target Group
Tạo các nhóm đích để Load Balancer chuyển tiếp traffic:
* `TG-Backend-Blue`: Cổng port `5000`, target type `IP`.
* `TG-Backend-Green`: Cổng port `5000`, target type `IP` (dành cho Blue/Green Deployment).
* `TG-Frontend`: Cổng port `80` (hoặc port ứng dụng), target type `IP`.

###### 7.2. Tạo Application Load Balancer
Tạo ALB mang tên `ECS-ALB` thuộc vùng mạng Public Subnets, gán Security Group `ALB-SG`.
* Listener 1: Port `80` mặc định chuyển tiếp traffic tới `TG-Frontend`.
* Listener 2: Port `8080` hoặc port API chuyển tiếp traffic tới `TG-Backend-Blue` (sử dụng thêm port phụ `8081` cho việc kiểm thử Green target).

##### 8. Khởi tạo ECS Service
ECS Service quản lý vòng đời và số lượng Task chạy trong cluster.

###### 8.1. Blue/Green Deployment và Service Scaling cho Backend
Khởi tạo ECS Service cho Backend chạy dưới dạng Fargate. Lựa chọn kiểu triển khai là **Blue/Green Deployment (powered by AWS CodeDeploy)**:
* Chọn Load Balancer `ECS-ALB` và Listener port liên quan.
* Gán 2 Target Group: `TG-Backend-Blue` và `TG-Backend-Green`.
* Cấu hình chính sách Auto Scaling tự động tăng giảm số lượng task dựa trên CPU/Memory Utilization.

###### 8.2. Triển khai Rolling với Frontend
Khởi tạo ECS Service cho Frontend sử dụng kiểu triển khai **Rolling Update (ECS Default)**. Thiết lập thông số:
* `Minimum healthy percent`: `100%`
* `Maximum percent`: `200%`
Đảm bảo khi cập nhật phiên bản mới, hệ thống luôn có ít nhất số lượng task cũ hoạt động trước khi hoàn tất thay thế các task mới.

##### 9. Kiểm tra Kết quả (Test Result)
* Truy cập địa chỉ DNS Name của Application Load Balancer trên trình duyệt.
* Xác nhận giao diện frontend tải thành công.
* Thực hiện tương tác dữ liệu để kiểm tra kết nối giữa Frontend và Backend.
* Thực hiện cập nhật image để chạy thử quy trình CodeDeploy Blue/Green deployment và xác nhận traffic được định tuyến mượt mà từ Blue sang Green.

##### 10. Dọn dẹp Tài nguyên (Clean Up Resources)
Xóa ECS Service, ECS Cluster, giải phóng Application Load Balancer, xóa các Target Group, NAT Gateway và Release Elastic IP để tối ưu hóa chi phí.

---

#### Lab 2: Tự động hóa CI/CD và Giám sát với CloudWatch & Firelens (Lab 17)

##### 1. Giới thiệu (Introduction)
Sau khi đã triển khai ứng dụng thủ công lên ECS, bài lab này hướng đến việc tự động hóa quy trình phân phối (CI/CD) sử dụng ba công cụ phổ biến: **GitLab CI/CD**, **GitHub Actions**, và **AWS CodeBuild**. Đồng thời cấu hình hệ thống giám sát hiệu năng với **CloudWatch Container Insights** và định tuyến Log nâng cao thông qua **AWS Firelens (Fluent Bit)**.

##### 2. Chuẩn bị (Preparation)
* Tạo các kho lưu trữ (repositories) chứa source code ứng dụng trên GitLab và GitHub.
* Thiết lập các IAM policies và IAM Roles cung cấp quyền truy cập cần thiết để đẩy images lên ECR và cập nhật ECS Service.

##### 3. Quy trình CI/CD với GitLab
Sử dụng GitLab CI/CD để tự động hóa build và deploy.

###### 3.1. Fork và Tùy chỉnh Repository
Thực hiện fork project mẫu về tài khoản GitLab cá nhân và chỉnh sửa tệp cấu hình `.gitlab-ci.yml`.

###### 3.2. Cài đặt và Đăng ký GitLab Runner
Khởi chạy một EC2 Instance đóng vai trò làm build agent, cài đặt GitLab Runner và thực hiện đăng ký (register) Runner với GitLab project sử dụng Registration Token:
```bash
sudo gitlab-runner register \
  --non-interactive \
  --url "https://gitlab.com/" \
  --registration-token "<project_token>" \
  --executor "shell" \
  --description "AWS-ECS-Runner"
```

###### 3.3. Cấu hình Quyền hạn và IAM Role
Gán IAM Role cho EC2 Instance (GitLab Runner) với các quyền cụ thể liên quan đến Amazon ECR (`AmazonEC2ContainerRegistryPowerUser`) và Amazon ECS để Runner có thể gọi các API của AWS trực tiếp mà không cần lưu trữ Access Keys tĩnh trong container.

###### 3.4. Cấu hình Variables và Chạy thử Pipeline
Thêm các biến môi trường bảo mật trên giao diện GitLab Settings -> CI/CD -> Variables (như `AWS_DEFAULT_REGION`, `AWS_ACCOUNT_ID`). Thực hiện push commit mới lên nhánh `main` để kích hoạt pipeline.

###### 3.5. Kiểm tra Kết quả (Check Results)
Theo dõi trạng thái chạy của các stages (Build, Test, Deploy) trong tab Pipelines. Đảm bảo docker image mới được đẩy lên ECR và ECS Service thực hiện cập nhật task mới thành công.

##### 4. Quy trình CI/CD với GitHub Actions

###### 4.1. Clone GitHub Template
Clone kho lưu trữ chứa template cấu hình workflow về máy local.

###### 4.2. Khởi tạo Project và Đẩy mã nguồn lên GitHub
Tạo repository mới trên GitHub cá nhân, cấu hình Git remote và thực hiện push mã nguồn lên.

###### 4.3. Cấu hình Access key và Secret key
Tại phần Settings -> Secrets and variables -> Actions của GitHub project, thêm 2 secret key:
* `AWS_ACCESS_KEY_ID`
* `AWS_SECRET_ACCESS_KEY`
Các thông số này được sử dụng trong file workflow cấu hình `.github/workflows/aws.yml` để authenticate với AWS.

###### 4.4. Kiểm tra Kết quả (Check the Results)
Quan sát tiến trình chạy tại mục **Actions** của GitHub. Sau khi build hoàn tất, xác thực xem ECS Task Definition đã tự động cập nhật version mới và deploy thành công lên dịch vụ ECS.

##### 5. Quy trình CI/CD với AWS CodeBuild
Sử dụng công cụ CI/CD native của AWS.

###### 5.1. Cấu hình liên kết GitHub
Truy cập AWS CodeBuild Console, thực hiện kết nối tài khoản AWS với GitHub để CodeBuild có quyền truy cập mã nguồn.

###### 5.2. Khởi tạo dự án CodeBuild cho Frontend
Tạo build project `Frontend-Build`. Sử dụng file cấu hình `buildspec.yml` nằm trong thư mục nguồn để mô tả các bước cài đặt dependencies, build project và đóng gói Docker.

###### 5.3. Khởi tạo dự án CodeBuild cho Backend
Tạo build project `Backend-Build` tương tự cho dịch vụ Backend, cấu hình đẩy docker image sau khi build thành công lên repo ECR tương ứng.

###### 5.4. Cấu hình Trigger Tag
Cấu hình Webhook trong CodeBuild để tự động kích hoạt tiến trình build bất cứ khi nào có hành động push Git Tag mới (ví dụ: `v*.*.*`) lên repository.

###### 5.5. Xác minh Kết quả (Verify Results)
Kiểm tra lịch sử chạy trong mục Build History. Xác minh hình ảnh container mới đã được đẩy lên ECR và Task Definition được cập nhật.

##### 6. Giám sát ứng dụng với Container Insights (CloudWatch)
Kích hoạt tính năng **CloudWatch Container Insights** trên ECS Cluster `ECS-App-Cluster`. Giám sát các chỉ số chuyên sâu ở cấp độ container như:
* CPU/Memory Utilization của từng Service, Task.
* Network Rx/Tx.
* Số lượng Task đang chạy thực tế.
Vẽ các biểu đồ trực quan hóa dữ liệu hiệu năng trên CloudWatch Dashboard.

##### 7. Cấu hình Logs Router với Firelens
AWS Firelens cho phép định tuyến logs container (stdout/stderr) sang các dịch vụ lưu trữ khác bằng cách sử dụng Fluent Bit hoặc Fluentd làm sidecar container.

###### 7.1. Tạo S3 Bucket để lưu trữ Logs
Tạo một S3 Bucket mới với tên ví dụ `ecs-app-logs-bucket` để làm nơi lưu trữ logs lâu dài nhằm giảm thiểu chi phí lưu trữ trên CloudWatch Logs.

###### 7.2. Tạo IAM Role cho ECS Task
Thiết lập IAM Task Role và Task Execution Role, đính kèm policy cho phép ghi dữ liệu lên S3 Bucket (`s3:PutObject`).

###### 7.3. Cập nhật ECS Service
Chỉnh sửa Task Definition để thêm container phụ **log_router** chạy image Fluent Bit chuẩn của AWS. Cấu hình phần `logConfiguration` của container chính (Frontend/Backend) sử dụng driver `awsfirelens` để chuyển tiếp log qua Fluent Bit, từ đó đẩy thẳng về S3.

###### 7.4. Kiểm tra logs
Kiểm tra trên S3 Bucket để xác nhận cấu trúc folder logs đã được tạo tự động và chứa các tệp log xuất ra từ ứng dụng.

##### 8. Dọn dẹp Tài nguyên (Resource Cleanup)
Xóa các dự án CodeBuild, xóa GitLab Runner trên EC2, tắt máy ảo EC2, xóa S3 Bucket lưu logs và tắt Container Insights để tránh phát sinh chi phí ngoài ý muốn.

---

#### Lab 3: Đánh giá an toàn hệ thống với AWS Security Hub (Lab 18)

##### 1. Tiêu chuẩn bảo mật (Security Standards)
**AWS Security Hub** cung cấp một cái nhìn toàn diện về trạng thái bảo mật của toàn bộ tài khoản AWS của bạn bằng cách liên tục kiểm tra tài nguyên hệ thống dựa trên các bộ tiêu chuẩn công nghiệp. Hai bộ tiêu chuẩn chính được sử dụng bao gồm:
* **AWS Foundational Security Best Practices (FSBP)**: Tập hợp các quy tắc bảo mật được thiết kế bởi AWS.
* **CIS AWS Foundations Benchmark v1.2.0 & v1.4.0**: Các hướng dẫn bảo mật khách quan, đồng thuận từ các chuyên gia bảo mật.

##### 2. Kích hoạt Security Hub
* Truy cập vào AWS Security Hub Console.
* Nhấp chọn **Enable Security Hub**.
* Lựa chọn kích hoạt các tiêu chuẩn bảo mật mong muốn (FSBP và CIS). Quá trình quét và tổng hợp dữ liệu ban đầu sẽ mất vài giờ để hoàn thành.

##### 3. Đánh giá Điểm số bảo mật theo Tiêu chuẩn (Security Score by Standards)
* Theo dõi mục **Security Score** trên Dashboard để nắm bắt tỷ lệ phần trăm tuân thủ an toàn thông tin của tài khoản.
* Phân tích các phát hiện (Findings) được phân loại theo mức độ nghiêm trọng (Critical, High, Medium, Low).
* Đọc các khuyến nghị sửa lỗi (Remediation) đi kèm cho từng tài nguyên vi phạm (ví dụ: MFA chưa bật cho tài khoản root, Security Group mở cổng port 22 quá rộng, S3 bucket bật Public Access...).

##### 4. Dọn dẹp tài nguyên (Clean Up Resources)
Sau khi hoàn tất quá trình đánh giá và ghi nhận kết quả cho báo cáo thực tập, thực hiện tắt/vô hiệu hóa AWS Security Hub để tránh phát sinh chi phí duy trì dịch vụ liên tục.

