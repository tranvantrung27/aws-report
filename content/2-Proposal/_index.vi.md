---
title: "Bản đề xuất"
date: 2026-04-26
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Nền tảng IoT Giám sát Thời tiết Thời gian thực với AWS Serverless

> **ITea Lab** — AWS Cloud (Asia Pacific · Singapore) · Infrastructure as Code với AWS CDK

---

## Bối cảnh & Vấn đề

Phòng lab *ITea Lab* hiện đang vận hành nhiều trạm thời tiết vật lý, nhưng toàn bộ quá trình thu thập và báo cáo dữ liệu đều thực hiện thủ công. Khi số lượng trạm tăng lên, việc quản lý trở nên không khả thi — không có luồng dữ liệu tự động, không có dashboard tập trung, và không có cơ chế giám sát hay cảnh báo khi có sự cố.

Các nền tảng IoT thương mại như **Thingsboard** hay **CoreIoT** cung cấp đầy đủ tính năng, nhưng lại quá nặng và tốn kém so với nhu cầu thực tế của một phòng lab nghiên cứu quy mô nhỏ với 5 thành viên.

---

## Giải pháp: Xây dựng nền tảng AWS Serverless riêng

Thay vì dùng giải pháp bên thứ ba, chúng tôi quyết định tự xây dựng một nền tảng nhẹ, chi phí thấp và hoàn toàn tự động hóa trên AWS — đủ đáp ứng nhu cầu của phòng lab, đồng thời là cơ sở để mở rộng thành hệ thống lớn hơn trong tương lai.

Toàn bộ hạ tầng được định nghĩa và triển khai bằng **AWS CDK (Infrastructure as Code)**, đảm bảo tính nhất quán và khả năng tái triển khai nhanh.

---

## Kiến trúc hệ thống

Hệ thống gồm **5 trạm thời tiết** (Raspberry Pi + ESP32), có thể mở rộng lên 15 trạm. Dữ liệu cảm biến (nhiệt độ, độ ẩm, gió, mưa) được truyền qua **MQTT** với xác thực **X.509 Certificate** đến **AWS IoT Core**. Từ đó, **IoT Core Rule** tự động định tuyến vào pipeline xử lý trên cloud.

*Sơ đồ kiến trúc tổng thể (Asia Pacific · Singapore Region):*

![AWS Serverless Architecture — IoT Weather Platform (ITea Lab)](/images/2-Proposal/IOT.jpg)

**Luồng dữ liệu chính:**

```
Cảm biến (ESP32)
    → Raspberry Pi Edge Device
        → MQTT / X.509 → AWS IoT Core (MQTT Broker)
            → IoT Core Rule → Amazon S3 Raw Data Lake
                → AWS Lambda (Trigger) → AWS Glue Crawler
                    → AWS Glue ETL Job → S3 Processed Analytics Data
                        → API Gateway + Lambda → Next.js Dashboard (Inspira)
                                                    → AWS Amplify + Cognito
```

**Các dịch vụ AWS sử dụng:**

| Dịch vụ | Vai trò trong hệ thống |
| :--- | :--- |
| **AWS IoT Core** | MQTT Broker — tiếp nhận dữ liệu từ 5 trạm, bảo mật X.509 |
| **IoT Core Rule** | Định tuyến tự động dữ liệu thô vào S3 Raw Data Lake |
| **Amazon S3 (x2)** | Raw Data Lake + S3 Processed Analytics Data |
| **S3 Lifecycle Policy + Glacier** | Tự động chuyển dữ liệu cũ sang Glacier để lưu trữ dài hạn tiết kiệm chi phí |
| **AWS Lambda** | Process & Trigger Glue — kích hoạt pipeline xử lý dữ liệu |
| **AWS Glue Crawler + ETL Job** | Lập chỉ mục và chuyển đổi dữ liệu thô thành dữ liệu phân tích |
| **Amazon API Gateway** | REST API cung cấp dữ liệu cho web dashboard |
| **Next.js + AWS Amplify** | Web dashboard (Inspira template) — hiển thị real-time |
| **Amazon Cognito** | Xác thực người dùng — giới hạn 5 Lab Members |
| **CloudWatch + SNS** | Giám sát log Lambda/Glue; cảnh báo Lambda Errors, Message Count Drop, Budget → Email/SMS Admin |
| **AWS CDK** | Infrastructure as Code — toàn bộ hạ tầng được quản lý bằng code |

---

## Kế hoạch triển khai

| Giai đoạn | Thời gian | Nội dung |
| :--- | :--- | :--- |
| Nghiên cứu & Thiết kế | Tháng 0 (trước thực tập) | Nghiên cứu phần cứng, thiết kế kiến trúc, viết CDK stacks |
| Tính toán & Điều chỉnh | Tháng 1 | AWS Pricing Calculator, học dịch vụ liên quan, nâng cấp phần cứng |
| Phát triển | Tháng 2 | Lập trình Raspberry Pi, triển khai AWS với CDK, xây dựng Next.js dashboard |
| Kiểm thử & Ra mắt | Tháng 3 | End-to-end testing, cấu hình CloudWatch Alarms, triển khai production |
| Thu thập dữ liệu | 1 năm sau | Vận hành liên tục để tích lũy dữ liệu thời tiết cho nghiên cứu AI/ML |

---

## Chi phí ước tính

Chi phí vận hành hàng tháng cực kỳ thấp nhờ kiến trúc Serverless — chỉ trả tiền khi có dữ liệu gửi đến.

| Dịch vụ | Chi phí/tháng |
| :--- | :--- |
| AWS IoT Core (MQTT) | $0.08 |
| Amazon S3 (x2 buckets) | $0.15 |
| AWS Glue Crawler | $0.07 |
| AWS Glue ETL Jobs | $0.02 |
| AWS Lambda | $0.00 |
| Amazon API Gateway | $0.01 |
| AWS Amplify | $0.35 |
| Data Transfer | $0.02 |
| **Tổng** | **~$0.70/tháng · $8.40/năm** |

> Phần cứng ($265 — Raspberry Pi 5 + cảm biến) được tận dụng từ hệ thống trạm thời tiết sẵn có.
> Chi phí đầy đủ: [AWS Pricing Calculator](https://calculator.aws/#/estimate?id=621f38b12a1ef026842ba2ddfe46ff936ed4ab01)

---

## Rủi ro & Phương án dự phòng

| Rủi ro | Phương án xử lý |
| :--- | :--- |
| Mất kết nối mạng tại trạm | Docker buffer cục bộ trên Raspberry Pi — dữ liệu được gửi lại khi kết nối phục hồi |
| Hỏng cảm biến ESP32 | Kiểm tra định kỳ + dự phòng linh kiện thay thế |
| Chi phí AWS vượt dự kiến | CloudWatch Budget Alarm tự động cảnh báo trước khi đến ngưỡng |
| Lỗi Lambda hoặc Glue pipeline | CloudWatch Alarms + SNS gửi thông báo ngay cho Admin để xử lý |
| Sự cố nghiêm trọng AWS | Toàn bộ hạ tầng tái triển khai nhanh bằng AWS CDK từ code |

---

## Kết quả kỳ vọng

Sau khi hoàn thành, hệ thống sẽ:
- **Tự động hóa hoàn toàn** luồng dữ liệu từ cảm biến đến dashboard, không cần can thiệp thủ công
- **Cung cấp dữ liệu thời tiết liên tục** trong 1 năm — nguồn tài nguyên phục vụ nghiên cứu AI/ML của phòng lab
- **Có khả năng mở rộng** từ 5 lên 10–15 trạm mà không cần thay đổi kiến trúc
- **Trở thành nền tảng tham khảo** để xây dựng các hệ thống IoT lớn hơn trong tương lai