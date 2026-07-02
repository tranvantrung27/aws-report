---
title : "Tổng quan về Workshop"
date : 2026-04-26
weight : 1
chapter : false
pre : " <b> 5.1. </b> "
---

#### Tổng quan

**IoT Weather Platform** là hệ thống giám sát thời tiết thời gian thực, được xây dựng trên nền AWS Serverless dành cho nhóm nghiên cứu *ITea Lab* tại TP. Hồ Chí Minh.

Hệ thống bao gồm hai phần chính:
- **Edge Station**: Trạm thời tiết biên sử dụng Raspberry Pi + cảm biến ESP32 thu thập dữ liệu nhiệt độ, độ ẩm, lượng mưa, tốc độ gió và truyền về qua giao thức MQTT.
- **Cloud Platform**: Hạ tầng AWS xử lý, lưu trữ và trực quan hóa dữ liệu thu thập được.

#### Kiến trúc hệ thống

![IoT Weather Platform Architecture](/images/2-Proposal/platform_architecture.jpeg)

#### Dịch vụ AWS sử dụng

| Dịch vụ | Vai trò |
| :--- | :--- |
| **AWS IoT Core** | Tiếp nhận dữ liệu MQTT từ các trạm |
| **AWS Lambda** | Xử lý dữ liệu và kích hoạt Glue jobs |
| **Amazon API Gateway** | Giao tiếp với ứng dụng web |
| **Amazon S3** | Lưu trữ thô (data lake) và dữ liệu đã xử lý |
| **AWS Glue** | Crawlers + ETL jobs chuyển đổi dữ liệu |
| **AWS Amplify** | Hosting giao diện web Next.js |
| **Amazon Cognito** | Quản lý xác thực người dùng |
