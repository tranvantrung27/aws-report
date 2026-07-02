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
* Cài đặt phần mềm ảo hóa **VMware Workstation Pro** trên môi trường Local (On-premises).
* Tạo một máy ảo chạy hệ điều hành **Ubuntu Desktop** cấu hình ổ đĩa **20 GB** dưới dạng một tệp ảo hóa duy nhất (*single file*).
* Cài đặt và cấu hình SSH Server để hỗ trợ kết nối từ xa:
```bash
sudo apt update
sudo apt install openssh-server -y
sudo systemctl enable ssh --now
```

##### 2. Import máy ảo vào AWS (Import VM to AWS)
###### 2.1 Export máy ảo từ On-premise
* Tắt máy ảo Ubuntu trên VMware.
* Chọn máy ảo, vào menu **File** -> **Export to OVF...**
* Thu được tệp ổ đĩa ảo hóa dạng **`.vmdk`** (ví dụ: `Ubuntu-disk1.vmdk`).

###### 2.2 Tải máy ảo lên AWS
* Truy cập dịch vụ **Amazon S3** và tạo một bucket mới `vm-import-export-bucket-trung-2026` bật tùy chọn **ACLs enabled** và tắt chặn quyền truy cập công khai (Block Public Access = Off).
* Upload tệp tin `Ubuntu-disk1.vmdk` lên bucket vừa tạo.
* Minh chứng cấu hình bucket cho phép ACLs:

![S3 ACL Enabled](/images/worklog/week-8/1_s3_acl_enabled.png)

###### 2.3 Import máy ảo vào AWS
* Cấu hình vai trò IAM Role có tên chính xác là `vmimport` để cấp quyền cho dịch vụ import.
* Khởi tạo tệp cấu hình quan hệ tin cậy `trust-policy.json`:
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
* Chạy câu lệnh tạo vai trò:
```bash
aws iam create-role --role-name vmimport --assume-role-policy-document "file://trust-policy.json"
```
* Gán chính sách phân quyền S3 và EC2 cho vai trò `role-policy.json`:
```bash
aws iam put-role-policy --role-name vmimport --policy-name vmimport-policy --policy-document "file://role-policy.json"
```
* Minh chứng khởi tạo thành công IAM Role `vmimport` với Trusted Entity chính xác:

![IAM Role vmimport](/images/worklog/week-8/1_iam_role_vmimport.png)

* Thiết lập file cấu hình `containers.json` chỉ định tệp nguồn và chạy lệnh bắt đầu tiến trình import:
```bash
aws ec2 import-image --description "Ubuntu App Server Import" --disk-containers "file://containers.json"
```
* Theo dõi trạng thái tác vụ cho tới khi hoàn tất và tạo ra AMI:
```bash
aws ec2 describe-import-image-tasks --import-task-ids <ImportTaskId>
```

###### 2.4 Triển khai EC2 Instance từ AMI
* Truy cập EC2 Console > mục **AMIs**, chọn AMI ID vừa được tạo ra.
* Nhấp **Launch instance từ AMI** để tạo máy chủ ảo EC2 `Import-Server` (chọn loại máy chủ `t3.micro` thuộc Free Tier được cho phép của tài khoản và mở cổng SSH 22).
* Mở terminal local và đăng nhập SSH kiểm tra ứng dụng:
```bash
ssh awsstudent@<Public_IP_EC2>
```

##### 3. Export EC2 Instance từ AWS (Export EC2 Instance from AWS)
###### 3.1 Thiết lập ACL cho S3 Bucket
* Để cho phép dịch vụ VM Import/Export ghi file máy ảo kết quả ngược lại S3 bucket, cần bổ sung quyền ghi cho Canonical ID dịch vụ tương ứng với Singapore Region:
```bash
aws s3api put-bucket-acl --bucket YOUR_BUCKET_NAME --grant-write "id=c4d8eabf8db69dbe46bfe0e517100c554f01200b104d59cd408e777ba442a322" --grant-read-acp "id=c4d8eabf8db69dbe46bfe0e517100c554f01200b104d59cd408e777ba442a322"
```

###### 3.2 Export máy ảo từ EC2 Instance
* Chạy lệnh xuất ngược máy ảo từ EC2 về S3 dưới dạng file định dạng `.ova` cho VMware:
```bash
aws ec2 create-instance-export-task --instance-id <InstanceId> --target-environment vmware --export-to-s3-task DiskImageFormat=vmdk,ContainerFormat=ova,S3Bucket=YOUR_BUCKET_NAME,S3Prefix=exports/
```

###### 3.3 Export máy ảo từ AMI
* Chạy lệnh xuất ngược trực tiếp từ AMI:
```bash
aws ec2 export-image --image-id <AMI_ID> --disk-image-format VMDK --s3-export-location S3Bucket=YOUR_BUCKET_NAME,S3Prefix=exports/
```

##### 4. Video tham khảo (Reference Video)
* Nghiên cứu và theo dõi các video mô phỏng tiến trình export/import máy ảo để nắm bắt luồng dữ liệu thực tế.

##### 5. Dọn dẹp tài nguyên (Clean up resources)
Thực hiện dọn dẹp sạch toàn bộ tài nguyên thử nghiệm để tránh phát sinh chi phí:
```bash
aws ec2 terminate-instances --instance-ids <InstanceId>
aws ec2 deregister-image --image-id <AMI_ID>
aws s3 rb s3://vm-import-export-bucket-trung-2026/ --force
aws iam delete-role --role-name vmimport
```

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

