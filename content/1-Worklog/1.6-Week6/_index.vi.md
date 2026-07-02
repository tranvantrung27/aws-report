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

#### 2. Thực hành với Hybrid DNS – Route 53 Resolver

##### **Khởi tạo hạ tầng (CloudFormation)**
* Tạo Key Pair `hybrid-DNS` và triển khai thành công stack CloudFormation `HybridDNS` bao gồm VPC đa AZ, 4 subnet (2 public / 2 private), Security Group và máy chủ Windows Server 2022 (RDGW).

![CloudFormation HybridDNS CREATE_COMPLETE](/images/worklog/week-6/2.2_cloudformation_complete.png)

##### **Triển khai Microsoft Active Directory**
* Triển khai thành công AWS Managed Microsoft AD (`onprem.example.com`) mô phỏng hệ thống DNS on-premises, trạng thái **Active**.

![Microsoft AD Active](/images/worklog/week-6/4_microsoft_ad_active.png)

##### **Cấu hình Route 53 Resolver**
* Tạo **Outbound Endpoint** (`outbound-endpoint`) gắn vào 2 private subnet, trạng thái **Operational** – dùng để chuyển tiếp DNS query từ AWS đến DNS on-premises.
* Tạo **Resolver Rule** (`onprem-rule`) loại FORWARD cho domain `onprem.example.com`, chỉ đến 2 IP DNS của AD và liên kết với VPC.
* Tạo **Inbound Endpoint** (`inbound-endpoint`) trạng thái **Operational** – cho phép DNS on-premises truy vấn các tài nguyên nội bộ AWS.

![Outbound Endpoint Operational](/images/worklog/week-6/5.1_outbound_endpoint.png)

![Resolver Rule onprem.example.com](/images/worklog/week-6/5.2_resolver_rule.png)

![Inbound Endpoint Operational](/images/worklog/week-6/5.3_inbound_endpoint.png)


