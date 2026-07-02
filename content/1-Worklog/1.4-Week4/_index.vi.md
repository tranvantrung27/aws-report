---
title: "Worklog Tuần 4"
date: 2026-05-11
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Mục tiêu tuần 4:

* Tìm hiểu dịch vụ Amazon Relational Database Service (Amazon RDS) trên AWS.
* Thực hành triển khai hệ thống mạng (VPC, Security Group, Subnet Group) cho cơ sở dữ liệu.
* Khởi tạo và cấu hình Amazon RDS database instance.
* Kết nối ứng dụng chạy trên EC2 với RDS database.
* Tìm hiểu cơ chế sao lưu (Backup), khôi phục (Restore) và các tính năng cao cấp (Multi-AZ, Read Replicas).

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Tìm hiểu tổng quan về Amazon RDS <br> - So sánh RDS với các dịch vụ lưu trữ khác (EC2 DB, DynamoDB, Redshift) | 11/05/2026 | 11/05/2026 | [Amazon RDS Documentation](https://docs.aws.amazon.com/rds/) |
| 3 | - Thiết lập môi trường mạng cho RDS: <br>&emsp; + Tạo VPC <br>&emsp; + Tạo Security Groups cho EC2 và RDS <br>&emsp; + Tạo DB Subnet Group | 12/05/2026 | 12/05/2026 | [VPC for RDS](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_VPC.WorkingWithRDSInstanceinaVPC.html) |
| 4 | - Thực hành khởi tạo Amazon RDS instance <br> - Cấu hình tham số và tùy chọn bảo mật cho Database | 13/05/2026 | 13/05/2026 | [Creating an RDS DB Instance](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_CreateDBInstance.html) |
| 5 | - Triển khai ứng dụng trên EC2 và kết nối với RDS <br> - Kiểm tra khả năng truy cập và quản lý dữ liệu qua Endpoint | 14/05/2026 | 14/05/2026 | [Connecting to RDS](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_GettingStarted.CreatingConnecting.MySQL.html) |
| 6 | - Tìm hiểu và thực hành Backup & Restore <br> - Thực hành Snapshot thủ công và tự động | 15/05/2026 | 15/05/2026 | [RDS Backup and Restore](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_CommonTasks.BackupRestore.html) |
| 7-CN | - Tìm hiểu Multi-AZ và Read Replicas <br> - Dọn dẹp tài nguyên để tối ưu chi phí | 16/05/2026 | 17/05/2026 | [High Availability (Multi-AZ)](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.MultiAZ.html) |

### Kết quả đạt được tuần 4:

#### Về lý thuyết:
* Nắm vững kiến thức về Amazon RDS: Lợi ích, các DB engine hỗ trợ, tính năng quản lý.
* Hiểu cách thức hoạt động của Multi-AZ và Read Replicas.
* Biết cách phân biệt khi nào sử dụng RDS, DynamoDB, Redshift hay S3.
* Hiểu về cơ chế bảo mật (Security Group, Encryption) và sao lưu (Snapshot) trên RDS.

#### Về thực hành:
* Thiết lập thành công môi trường mạng an toàn cho Database (DB Subnet Group, Security Groups).
* Khởi tạo thành công Amazon RDS instance (MySQL/MariaDB).
* Kết nối thành công ứng dụng Web trên EC2 với RDS database thông qua Endpoint.
* Thực hiện thành công việc sao lưu và khôi phục dữ liệu bằng Snapshot.
* Quản lý tài nguyên hiệu quả, biết cách dọn dẹp để tránh phát sinh chi phí.

---

### Chi tiết thực hành Tuần 4:

#### 1. Tổng quan bài Lab
{{% notice info "Information" %}}
Trong bài lab này, chúng ta sẽ triển khai Amazon RDS (Relational Database Service) — dịch vụ cơ sở dữ liệu quan hệ được quản lý hoàn toàn bởi AWS. Chúng ta sẽ thiết lập môi trường mạng an toàn, khởi tạo RDS instance MySQL, kết nối ứng dụng Node.js đang chạy trên EC2 với RDS thông qua Endpoint, đồng thời thực hành Backup & Restore bằng Snapshot.
{{% /notice %}}

{{% notice warning "Security Note" %}}
RDS instance nên được đặt trong **private subnet** và chỉ cho phép kết nối từ EC2 instance thông qua Security Group — không nên bật Public access để đảm bảo bảo mật.
{{% /notice %}}

#### 2. Các bước chuẩn bị

##### 2.1 Tạo VPC
Thực hiện tạo VPC để thiết lập môi trường mạng cô lập cho các tài nguyên EC2 và RDS.
![Tạo VPC thành công](/images/worklog/week-4/1.png)
![Cấu hình VPC](/images/worklog/week-4/2.png)

##### 2.2 Tạo EC2 Security Group
Cấu hình Security Group cho EC2 để cho phép truy cập SSH (port 22) và HTTP (port 5000 cho ứng dụng Node.js).
![Tạo EC2 SG thành công](/images/worklog/week-4/3.png)

##### 2.3 Tạo RDS Security Group
{{% notice warning "Security Note" %}}
Security Group cho RDS nên được cấu hình chỉ cho phép Inbound traffic từ Security Group của EC2 Instance trên cổng của DB engine (ví dụ: 3306 cho MySQL).
{{% /notice %}}
![Tạo RDS SG thành công](/images/worklog/week-4/4.png)

##### 2.4 Tạo DB Subnet Group
{{% notice info "Information" %}}
Nhóm DB Subnet là một tập hợp các subnet (thường là riêng tư) mà bạn tạo trong một VPC và sau đó chỉ định cho các DB instances. Mỗi nhóm DB Subnet nên có các subnet trong ít nhất hai Availability Zone.
{{% /notice %}}
![Tạo DB Subnet Group thành công](/images/worklog/week-4/5.png)

#### 3. Tạo EC2 instance
{{% notice info %}}
Amazon Elastic Compute Cloud (EC2) cung cấp khả năng tính toán có thể điều chỉnh quy mô trong đám mây AWS. Sử dụng EC2 cho phép bạn triển khai các máy chủ ảo mà không cần đầu tư vào phần cứng vật lý.
{{% /notice %}}

**Các bước thực hiện:**
1. Truy cập **EC2 Console** -> **Launch Instance**.
2. Đặt tên Instance (ví dụ: `App-Server`).
3. Chọn **AMI**: Amazon Linux 2023 (Free tier eligible).
4. Chọn **Instance type**: `t2.micro` hoặc `t3.micro`.
5. Chọn **Key pair** để kết nối SSH.
6. Cấu hình **Network settings**: Chọn VPC đã tạo và Security Group cho EC2.
7. Chọn **Launch instance**.

![Khởi tạo EC2 thành công](/images/worklog/week-4/6.png)

**Kết nối SSH bằng MobaXterm:**
- Sử dụng Public IP của instance.
- User mặc định: `ec2-user`.
- Cung cấp file `.pem` trong phần Advanced SSH settings.

#### 4. Tạo RDS database instance
Thực hiện tạo một DB Instance MariaDB hoặc MySQL bằng Standard create.

1. Truy cập **RDS Console** -> **Create database**.
2. Chọn **Standard create**.
3. **Engine type**: MySQL (hoặc MariaDB).
4. **Templates**: Free Tier (để tránh phát sinh chi phí).
5. **Settings**:
    - DB instance identifier: `database-1`.
    - Master username: `admin`.
    - Master password: Cấu hình mật khẩu an toàn.
6. **Connectivity**:
    - Chọn VPC tương ứng.
    - Chọn **Existing VPC security groups**: Chọn RDS Security Group đã tạo.
7. Chọn **Create database**.

#### 5. Triển khai ứng dụng và kết nối RDS

##### Cài đặt Git và Node.js trên EC2
```bash
# Cập nhật hệ thống
sudo dnf update -y

# Cài đặt Git
sudo dnf install git -y

# Cài đặt NVM và Node.js
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
source ~/.nvm/nvm.sh
nvm install --lts
```

##### Triển khai ứng dụng
1. Clone repository:
   ```bash
   git clone https://github.com/AWS-First-Cloud-Journey/AWS-FCJ-Management
   cd AWS-FCJ-Management
   ```
2. Cài đặt phụ thuộc:
   ```bash
   npm install express dotenv express-handlebars body-parser mysql
   ```
3. Cấu hình môi trường (`.env`):
   ```bash
   DB_HOST="RDS_ENDPOINT"
   DB_NAME="first_cloud_users"
   DB_USER="admin"
   DB_PASS="YourPassword"
   ```

##### Khởi tạo dữ liệu RDS
Kết nối tới RDS từ EC2 và chạy script SQL:
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

-- Thêm dữ liệu mẫu
INSERT INTO `user` (`first_name`, `last_name`, `email`, `phone`, `comments`, `status`)
VALUES ('Amanda', 'Nunes', 'anunes@ufc.com', '012345 678910', 'I love AWS FCJ', 'active');
```

4. Chạy ứng dụng:
   ```bash
   npm start
   ```
Truy cập qua trình duyệt: `http://<EC2-Public-IP>:5000`.

#### 6. Backup và Restore
{{% notice info "Information" %}}
Amazon RDS cung cấp các tính năng backup tự động và thủ công, cho phép bạn khôi phục cơ sở dữ liệu về một thời điểm cụ thể hoặc từ một snapshot.
{{% /notice %}}

1. **Giám sát**: Kiểm tra tab **Monitoring** trên RDS console để xem CPU, Connections, IOPS.
2. **Tạo Snapshot**: Chọn DB Instance -> **Actions** -> **Take snapshot**.
3. **Restore Snapshot**:
   - Chọn Snapshot -> **Actions** -> **Restore snapshot**.
   - Cấu hình Instance mới và kiểm tra trạng thái khôi phục.

![Kiểm tra database đã restore](/images/worklog/week-4/7.png)
![Restore hoàn tất](/images/worklog/week-4/8.png)

#### 7. Dọn dẹp tài nguyên
{{% notice warning "Warning" %}}
Sau khi hoàn thành, hãy xóa tài nguyên để tránh phát sinh chi phí.
{{% /notice %}}

1. **Xóa RDS**: Chọn DB Instance -> **Delete** (Bỏ chọn Create final snapshot).
2. **Xóa DB Snapshot**: Chọn Snapshots -> **Delete**.
3. **Terminate EC2**: Chọn Instance -> **Instance state** -> **Terminate**.
4. **Giải phóng Elastic IP** (nếu có).
5. **Xóa VPC**: Xóa Security Groups, NAT Gateways trước, sau đó xóa VPC.
