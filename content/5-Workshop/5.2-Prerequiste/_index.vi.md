---
title : "Chuẩn bị môi trường"
date : 2026-04-26
weight : 2
chapter : false
pre : " <b> 5.2. </b> "
---

#### Yêu cầu trước khi bắt đầu

Trước khi triển khai, cần chuẩn bị đầy đủ các tài nguyên sau:

##### Phần cứng
- Raspberry Pi (đã cài Raspbian OS)
- Cảm biến ESP32 (nhiệt độ, độ ẩm, lượng mưa, tốc độ gió)
- Kết nối Wi-Fi ổn định

##### Tài khoản & quyền AWS
- Tài khoản AWS với quyền truy cập các dịch vụ: IoT Core, S3, Lambda, Glue, Amplify, Cognito, API Gateway

##### Công cụ phát triển
- AWS CLI đã cấu hình
- Node.js (cho ứng dụng Next.js)
- Python 3.x (cho Lambda và Glue scripts)
- Docker (chạy trên Raspberry Pi để lọc dữ liệu)

{{% notice info "Information" %}}
Nội dung chi tiết từng bước chuẩn bị sẽ được cập nhật khi triển khai thực tế.
{{% /notice %}}