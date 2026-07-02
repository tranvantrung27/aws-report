---
title : "Thiết lập AWS IoT Core & S3 Data Lake"
date : 2026-04-26
weight : 3
chapter : false
pre : " <b> 5.3. </b> "
---

#### Mục tiêu

Trong phần này, chúng ta sẽ thiết lập luồng dữ liệu từ các trạm thời tiết vào AWS:
- Cấu hình **AWS IoT Core** để tiếp nhận dữ liệu MQTT từ Raspberry Pi
- Tạo **IoT Rules** để tự động đẩy dữ liệu vào S3
- Thiết lập **S3 Data Lake** (bucket lưu trữ dữ liệu thô)

{{% notice info "Information" %}}
Nội dung chi tiết từng bước thực hiện sẽ được cập nhật khi triển khai thực tế.
{{% /notice %}}

#### Nội dung

1. [Tạo IoT Core Thing & Certificate](5.3.1-create-gwe/)
2. [Kiểm tra kết nối MQTT](5.3.2-test-gwe/)