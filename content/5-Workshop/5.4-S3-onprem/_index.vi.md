---
title : "Xử lý dữ liệu với AWS Glue"
date : 2026-04-26
weight : 4
chapter : false
pre : " <b> 5.4. </b> "
---

#### Mục tiêu

Trong phần này, chúng ta sẽ xây dựng pipeline xử lý dữ liệu tự động:
- Cấu hình **AWS Glue Crawler** để lập chỉ mục dữ liệu từ S3 Data Lake
- Viết và chạy **ETL Job** để chuyển đổi dữ liệu thô sang định dạng phân tích
- Lưu kết quả vào **S3 bucket** thứ hai phục vụ dashboard

{{% notice info "Information" %}}
Nội dung chi tiết từng bước thực hiện sẽ được cập nhật khi triển khai thực tế.
{{% /notice %}}

#### Nội dung

1. [Chuẩn bị Glue Crawler](5.4.1-prepare/)
2. [Tạo Interface Endpoint](5.4.2-create-interface-enpoint/)
3. [Kiểm tra Endpoint](5.4.3-test-endpoint/)
4. [DNS Simulation](5.4.4-dns-simulation/)
