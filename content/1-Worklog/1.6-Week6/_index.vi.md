---
title: "Worklog Tuần 6"
date: 2026-05-25
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Mục tiêu tuần 6:

* Giám sát hệ thống AWS với **Amazon CloudWatch**: Metrics, Logs, Alarms và Dashboards.
* Tìm hiểu các gói hỗ trợ của AWS và cách quản lý yêu cầu hỗ trợ qua **AWS Support**.
* Triển khai hạ tầng Hybrid DNS tích hợp on-premises với AWS bằng **Route 53 Resolver** và **Microsoft AD**.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Tìm hiểu Amazon CloudWatch & chuẩn bị môi trường <br> - **CloudWatch Metrics:** <br>&emsp; + Xem các Metrics <br>&emsp; + Thực hiện các phép tìm kiếm <br>&emsp; + Thực hiện các phép toán học <br>&emsp; + Tạo dynamic labels | 25/05/2026 | 25/05/2026 | [What is Amazon CloudWatch?](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html) <br> [Using Amazon CloudWatch Metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/working_with_metrics.html) |
| 3 | - **CloudWatch Logs:** <br>&emsp; + Xem CloudWatch Logs <br>&emsp; + CloudWatch Logs Insights <br>&emsp; + CloudWatch Metric Filter <br> - Tạo CloudWatch Alarms <br> - Tạo CloudWatch Dashboards <br> - Dọn dẹp tài nguyên | 26/05/2026 | 26/05/2026 | [What is Amazon CloudWatch Logs?](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html) <br> [Creating CloudWatch Alarms](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html) <br> [CloudWatch Dashboards](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Dashboards.html) |
| 4 | - Tìm hiểu các gói hỗ trợ của AWS: Basic, Developer, Business, Enterprise On-Ramp, Enterprise <br> - Truy cập AWS Support Console <br> - Tìm hiểu các loại Yêu cầu Hỗ trợ (Account & billing, Technical) <br> - Thay đổi gói hỗ trợ | 27/05/2026 | 27/05/2026 | [AWS Support Plans](https://aws.amazon.com/premiumsupport/plans/) <br> [Getting started with AWS Support](https://docs.aws.amazon.com/awssupport/latest/user/getting-started.html) |
| 5 | - Khởi tạo Yêu cầu Hỗ trợ (Create Support Case) <br> - Lựa chọn mức độ nghiêm trọng (Severity) <br> - Quản lý và theo dõi trạng thái yêu cầu hỗ trợ | 28/05/2026 | 28/05/2026 | [Creating a support case](https://docs.aws.amazon.com/awssupport/latest/user/case-management.html) |
| 6 | - Đọc tổng quan bài lab Hybrid DNS <br> - Tạo Key Pair <br> - Khởi tạo CloudFormation Template <br> - Cấu hình Security Group <br> - Kết nối RDGW (Remote Desktop Gateway) <br> - Triển khai Microsoft Active Directory | 29/05/2026 | 29/05/2026 | [What is AWS Directory Service?](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/what_is.html) <br> [AWS CloudFormation User Guide](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html) |
| 7 - CN | - Thiết lập DNS với Route 53: <br>&emsp; + Tạo Route 53 Outbound Endpoint <br>&emsp; + Tạo Route 53 Resolver Rules <br>&emsp; + Tạo Route 53 Inbound Endpoints <br>&emsp; + Kiểm tra kết quả <br> - Dọn dẹp tài nguyên | 30/05/2026 | 31/05/2026 | [Forwarding outbound DNS queries](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resolver-forwarding-outbound-queries.html) <br> [Forwarding inbound DNS queries](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resolver-forwarding-inbound-queries.html) |

### Kết quả đạt được tuần 6:

#### 1. Thực hành với Amazon CloudWatch Workshop

##### **Khởi tạo môi trường (CloudFormation)**
* Triển khai thành công template CloudFormation tạo cấu trúc VPC, EC2 Instances và các chính sách IAM liên quan.
* Đã cấu hình và kích hoạt trình tạo log tự động `logger.py` trên EC2 Instance A bằng AWS Systems Manager (SSM) Run Command.
* Giám sát quá trình khởi tạo stack thành công trên AWS Console.

![Khởi tạo Stack thành công](/images/worklog/week-6/2.8.png)

##### **Thao tác với CloudWatch Metrics**
* Thực hiện so sánh hiệu suất sử dụng tài nguyên giữa các instance, cấu hình hiển thị hai đơn vị đo khác nhau trên hai trục Y độc lập (Left Y-axis và Right Y-axis).
* Thiết lập các vạch cảnh báo trực quan gồm Horizontal Annotation (ngưỡng 5% cho CPU) và Vertical Annotation (sự kiện "Job start" lúc 02:40).

![Thao tác với CloudWatch Metrics & Annotations](/images/worklog/week-6/3.1.19.png)

##### **Quản lý CloudWatch Logs & Logs Insights**
* Cập nhật thời hạn lưu trữ log cho nhóm `/ec2/linux/var/log/messages` từ *Never expire* thành *1 week (7 days)* nhằm tối ưu chi phí.
* Sử dụng Logs Insights chạy truy vấn lọc riêng các sự kiện chứa lỗi `ERROR`.
* Tạo thành công bộ lọc `PythonAppErrors` (Metric Filter) để chuyển đổi từ khóa lỗi từ log events thành số liệu giám sát.

![Cấu hình Metric Filter từ Logs](/images/worklog/week-6/4.3.8.png)

##### **Thiết lập CloudWatch Alarm & SNS Notification**
* Khởi tạo SNS Topic `Error_logs_reach_10` và đăng ký nhận thông báo email tự động.
* Cấu hình CloudWatch Alarm `PythonApplicationErrorAlarm` kích hoạt cảnh báo khi tổng số lỗi vượt quá 10 lỗi trong vòng 1 phút.

![Cấu hình CloudWatch Alarm thành công](/images/worklog/week-6/5.14.png)

##### **Thiết lập CloudWatch Dashboard**
* Xây dựng Dashboard `CloudWatch-Workshop` trực quan tích hợp cả biểu đồ lỗi Metric Filter và widget theo dõi trạng thái Alarm giúp quản trị viên có góc nhìn tổng quan và nhanh chóng nhất.

![Thiết lập Dashboard](/images/worklog/week-6/6.5.png)

#### 2. Tìm hiểu AWS Support

##### **Các gói hỗ trợ của AWS**
* Nghiên cứu 5 gói hỗ trợ của AWS theo thứ tự từ cơ bản đến nâng cao:
  * **Basic** – Miễn phí, chỉ hỗ trợ tài khoản và thanh toán.
  * **Developer** – Hỗ trợ kỹ thuật qua email (giờ hành chính).
  * **Business** – Hỗ trợ 24/7 qua phone, chat, email; SLA phản hồi 1 giờ.
  * **Enterprise On-Ramp** – Có Technical Account Manager (TAM) theo nhóm.
  * **Enterprise** – TAM chuyên biệt, SLA phản hồi 15 phút cho critical.
* Truy cập **AWS Support Console** và tìm hiểu giao diện quản lý Support Cases.
* Thực hiện xem xét và thay đổi gói hỗ trợ hiện tại.

##### **Khởi tạo Support Case**
* Tạo Support Case mới với các loại: **Account & Billing** (vấn đề tài khoản, hóa đơn) và **Technical** (vấn đề kỹ thuật với dịch vụ AWS).
* Lựa chọn mức độ nghiêm trọng (**Severity**) phù hợp: General guidance, System impaired, Production system impaired, Production system down, Business/Mission critical system down.
* Theo dõi trạng thái yêu cầu hỗ trợ và quản lý phản hồi từ AWS Support team.

#### 3. Thực hành với Hybrid DNS – Route 53 Resolver



##### **2.1 Tạo Key Pair**
* Tạo Key Pair `hybrid-DNS` (RSA, định dạng `.pem`) dùng để giải mã mật khẩu Administrator khi kết nối vào RDGW instance.

##### **2.2 Khởi tạo CloudFormation Template**
* Tải template từ GitHub repository và tạo stack CloudFormation `HybridDNS` với các thông số: Availability Zones `ap-southeast-1a` & `ap-southeast-1c`, Key Pair `hybrid-DNS`, Instance Type `t3.micro`.
* Stack khởi tạo thành công (`CREATE_COMPLETE`) bao gồm VPC đa AZ, 2 public subnet, 2 private subnet, Internet Gateway, Route Table, Security Group và EC2 instance RDGW chạy Windows Server 2022.

![CloudFormation HybridDNS CREATE_COMPLETE](/images/worklog/week-6/2.2_cloudformation_complete.png)

##### **2.3 Cấu hình Security Group**
* Truy cập Security Group có mô tả *"Enable RDP access from Internet"*.
* Xóa các rule không cần thiết (Port 3391, Port 443).
* Giới hạn rule **RDP (3389)** và **ICMP** chỉ cho phép từ IP của máy thực hành (`0.0.0.0/0` trong phạm vi lab) thay vì mở toàn bộ Internet.

##### **3. Kết nối RDGW (Remote Desktop Gateway)**
* Tải file RDP từ EC2 Console và giải mã mật khẩu Administrator bằng file `hybrid-DNS.pem`.
* Kết nối thành công vào máy chủ Windows Server 2022 qua Remote Desktop Protocol (RDP) với địa chỉ IP public `54.179.30.229`.

##### **4. Triển khai Microsoft Active Directory**
* Triển khai **AWS Managed Microsoft AD** với thông tin:
  * **Directory DNS name:** `onprem.example.com` (mô phỏng hệ thống DNS on-premises)
  * **NetBIOS name:** `onprem`
  * **Edition:** Standard
  * **VPC & Subnets:** Private subnet 1A (`ap-southeast-1a`) và Private subnet 2A (`ap-southeast-1b`)
* Sau ~20 phút, trạng thái chuyển sang **Active**, nhận được 2 địa chỉ DNS IP: `10.0.25.100` và `10.0.44.109`.

![Microsoft AD Active](/images/worklog/week-6/4_microsoft_ad_active.png)

##### **5.1 Tạo Route 53 Outbound Endpoint**
* Tạo Outbound Endpoint `outbound-endpoint` (IPv4, Do53) gắn vào 2 private subnet (`ap-southeast-1a` và `ap-southeast-1b`), trạng thái **Operational**.
* Endpoint này cho phép Route 53 Resolver chuyển tiếp DNS query từ AWS ra hệ thống DNS on-premises (AWS Managed Microsoft AD).

![Outbound Endpoint Operational](/images/worklog/week-6/5.1_outbound_endpoint.png)

##### **5.2 Tạo Route 53 Resolver Rules**
* Tạo Resolver Rule `onprem-rule` loại **FORWARD** cho domain `onprem.example.com`:
  * **Outbound Endpoint:** `outbound-endpoint`
  * **Target IP:** `10.0.25.100:53` và `10.0.44.109:53` (2 DNS IP của AD)
* Liên kết Rule với VPC `HybridDNS` – mọi DNS query cho `onprem.example.com` trong VPC sẽ được chuyển tiếp đến AD DNS server.

![Resolver Rule onprem.example.com](/images/worklog/week-6/5.2_resolver_rule.png)

##### **5.3 Tạo Route 53 Inbound Endpoint**
* Tạo Inbound Endpoint `inbound-endpoint` (IPv4, Do53) gắn vào 2 private subnet, trạng thái **Operational**.
* Endpoint này cho phép hệ thống DNS on-premises (AWS Managed Microsoft AD) gửi DNS query ngược lại vào Route 53 Resolver để phân giải các Private Hosted Zone trong AWS.

![Inbound Endpoint Operational](/images/worklog/week-6/5.3_inbound_endpoint.png)

##### **5.4 Kiểm tra kết quả**
* Từ máy chủ RDGW, chạy lệnh `nslookup onprem.example.com` và `Resolve-DnsName onprem.example.com` trên PowerShell để xác minh DNS resolution hoạt động đúng qua hệ thống Hybrid DNS đã cấu hình.

##### **6. Dọn dẹp tài nguyên**
* Xóa theo đúng thứ tự: **Inbound Endpoint** → **Disassociate & xóa Resolver Rule** → **Outbound Endpoint** → **AWS Managed Microsoft AD** → **CloudFormation Stack HybridDNS** → **Key Pair**.
