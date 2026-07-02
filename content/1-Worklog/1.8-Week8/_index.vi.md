---
title: "Worklog Tuần 8"
date: 2026-06-08
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Mục tiêu tuần 8:

* Tìm hiểu và thực hành dịch chuyển máy ảo từ On-premise lên AWS bằng dịch vụ **VM Import/Export**.
* Thực hành đóng gói ứng dụng bằng **Docker** và quản lý container với **Docker Compose** trên **Amazon EC2**.
* Cấu hình kết nối container ứng dụng với **Amazon RDS** và lưu trữ ảnh container trên **Amazon ECR** và **Docker Hub**.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Tìm hiểu lý thuyết về VM Import/Export <br> - **Lab 14:** Chuẩn bị máy ảo và export từ hệ thống On-premise | 08/06/2026 | 08/06/2026 | [VM Import/Export Requirements](https://docs.aws.amazon.com/vm-import/latest/userguide/vmie_prereqs.html) |
| 3 | - **Lab 14:** Tải file máy ảo lên Amazon S3 <br> - Thực hiện lệnh CLI để import máy ảo thành AMI và khởi chạy EC2 | 09/06/2026 | 09/06/2026 | [Importing a VM as an Image](https://docs.aws.amazon.com/vm-import/latest/userguide/vmimport-image.html) |
| 4 | - **Lab 14:** Cấu hình S3 Bucket ACL <br> - Export EC2 Instance/AMI từ AWS ngược về S3 dưới dạng file máy ảo và dọn dẹp tài nguyên | 10/06/2026 | 10/06/2026 | [Exporting an Instance/Image](https://docs.aws.amazon.com/vm-import/latest/userguide/vmexport.html) |
| 5 | - **Lab 15:** Triển khai thử nghiệm ứng dụng ở Local <br> - Chuẩn bị môi trường AWS: Cấu hình VPC, Security Group, tạo IAM Roles và login Docker Hub | 11/06/2026 | 11/06/2026 | [Docker Documentation](https://docs.docker.com/) <br> [Amazon ECR IAM Policies](https://docs.aws.amazon.com/AmazonECR/latest/userguide/security_iam_id-based-policy-examples.html) |
| 6 | - **Lab 15:** Tạo DB Subnet Group và khởi chạy RDS Instance <br> - Khởi chạy EC2 Instance làm Docker host và cài đặt các thư viện/cơ sở dữ liệu kiểm thử | 12/06/2026 | 12/06/2026 | [Amazon RDS DB Instance Creation](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_CreateDBInstance.html) <br> [Docker on AWS EC2](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/docker-wrapper.html) |
| 7 - CN | - **Lab 15:** Build Docker Image ứng dụng, chạy container và cấu hình Docker Compose trên EC2 kết nối với RDS <br> - Push Docker Image lên Amazon ECR và Docker Hub <br> - Dọn dẹp tài nguyên tránh phát sinh chi phí | 13/06/2026 | 14/06/2026 | [Docker Compose Reference](https://docs.docker.com/compose/) <br> [ECR Push Image Commands](https://docs.aws.amazon.com/AmazonECR/latest/userguide/docker-push-ecr-image.html) |

### Kết quả đạt được tuần 8:

---

### Chi tiết thực hành Tuần 8:

#### Lab 1: VM Import/Export (Lab 14)
##### 1. Chuẩn bị máy ảo (Prepare VM)
##### 2. Import máy ảo vào AWS (Import VM to AWS)
###### 2.1 Export máy ảo từ On-premise
###### 2.2 Tải máy ảo lên AWS
###### 2.3 Import máy ảo vào AWS
###### 2.4 Triển khai EC2 Instance từ AMI
##### 3. Export EC2 Instance từ AWS (Export EC2 Instance from AWS)
###### 3.1 Thiết lập ACL cho S3 Bucket
###### 3.2 Export máy ảo từ EC2 Instance
###### 3.3 Export máy ảo từ AMI
##### 4. Video tham khảo (Reference Video)
##### 5. Dọn dẹp tài nguyên (Clean up resources)

---

#### Lab 2: Deploy App with Docker / Docker Compose / RDS / ECR (Lab 15)
##### 1. Giới thiệu (Introduction)
##### 2. Triển khai trên Local (Deploy Local)
###### 2.1. Cài đặt Dependencies
###### 2.2. Triển khai ứng dụng
###### 2.3. Kiểm tra ứng dụng
##### 3. Chuẩn bị (Preparation)
###### 3.1. Cấu hình VPC
###### 3.2. Cấu hình Security Group
###### 3.3. Tạo IAM Roles để truy cập vào ECR
###### 3.4. Đăng nhập vào Docker Hub
##### 4. Khởi chạy RDS Instance (Launch RDS Instance)
###### 4.1. Tạo DB Subnet Group
###### 4.2. Khởi chạy RDS Instance
##### 5. Cấu hình EC2 Instance (Configure EC2 Instance)
###### 5.1. Cấu hình EC2 Instance
###### 5.2. Cài đặt các thư viện yêu cầu
###### 5.3. Thêm cơ sở dữ liệu để thử nghiệm
##### 6. Triển khai trên Docker Image (Deploy with Docker Image)
###### 6.1. Triển khai Application
###### 6.2. Kiểm tra Application
##### 7. Triển khai với Docker Compose (Deploy with Docker Compose)
###### 7.1. Triển khai Application
###### 7.2. Kiểm tra Application
##### 8. Push Image (Push Image)
###### 8.1. Sử dụng ECR
###### 8.2. Sử dụng Docker Hub
##### 9. Dọn Dẹp Tài Nguyên (Resource Cleanup)

