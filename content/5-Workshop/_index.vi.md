---
title: "Workshop"
date: 2026-04-26
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# IoT Weather Platform — Hướng dẫn triển khai chi tiết

#### Tổng quan

**IoT Weather Platform** là nền tảng giám sát thời tiết thời gian thực được xây dựng trên kiến trúc AWS Serverless dành cho nhóm *ITea Lab*. Hệ thống thu thập dữ liệu từ các trạm thời tiết sử dụng Raspberry Pi + ESP32, xử lý và lưu trữ qua các dịch vụ AWS, đồng thời trực quan hóa thông qua dashboard web.

Trong phần này, chúng ta sẽ đi qua toàn bộ các bước triển khai hệ thống từ đầu đến cuối — từ cấu hình phần cứng biên, thiết lập hạ tầng AWS, đến giao diện web và kiểm thử.

#### Nội dung

1. [Tổng quan về workshop](5.1-Workshop-overview/)
2. [Chuẩn bị môi trường](5.2-Prerequiste/)
3. [Thiết lập AWS IoT Core & S3 Data Lake](5.3-S3-vpc/)
4. [Xử lý dữ liệu với AWS Glue](5.4-S3-onprem/)
5. [Triển khai Web Dashboard với Amplify](5.5-Policy/)
6. [Dọn dẹp tài nguyên](5.6-Cleanup/)