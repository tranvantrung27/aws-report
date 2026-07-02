---
title : "Dọn dẹp tài nguyên"
date : 2026-04-26
weight : 6
chapter : false
pre : " <b> 5.6. </b> "
---

#### Dọn dẹp tài nguyên sau Lab

Sau khi hoàn thành workshop, thực hiện xóa các tài nguyên theo thứ tự sau để tránh phát sinh chi phí:

1. Xóa **AWS Amplify** app
2. Xóa **AWS Glue** Crawler và ETL Jobs
3. Xóa **S3 buckets** (data lake và processed bucket)
4. Xóa **AWS IoT Core** Things, Certificates, và Rules
5. Xóa **API Gateway** và **Lambda** functions
6. Xóa **Amazon Cognito** User Pool

{{% notice warning "Warning" %}}
Đảm bảo đã xóa **toàn bộ objects** bên trong S3 bucket trước khi xóa bucket, nếu không sẽ báo lỗi.
{{% /notice %}}

{{% notice tip "Pro Tip" %}}
Sử dụng **AWS Cost Explorer** để kiểm tra chi phí phát sinh sau khi dọn dẹp, đảm bảo không còn tài nguyên nào bị tính phí.
{{% /notice %}}