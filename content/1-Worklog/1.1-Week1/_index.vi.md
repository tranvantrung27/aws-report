---
title: "Worklog Tuần 1"
date: 2026-04-20
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---

### Mục tiêu tuần 1:

* Kết nối, làm quen với các thành viên trong First Cloud Journey.
* Hiểu dịch vụ AWS cơ bản, cách dùng console & CLI.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                             | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                         |
|:-----|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------|:-------------------|:------------------------------------------|
| 2   | - Làm quen với các thành viên FCJ <br> - Đọc và lưu ý các nội quy, quy định tại đơn vị thực tập                                                                                     | 20/04/2026 | 20/04/2026 |                                           |
| 3   | - Tìm hiểu AWS và các loại dịch vụ <br>&emsp; + Compute <br>&emsp; + Storage <br>&emsp; + Networking <br>&emsp; + Database <br>&emsp; + ... <br>                                                 | 21/04/2026 | 21/04/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - Tạo AWS Free Tier account <br> - Tìm hiểu AWS Console & AWS CLI <br> - **Thực hành:** <br>&emsp; + Tạo AWS account <br>&emsp; + Cài AWS CLI & cấu hình <br> &emsp; + Cách sử dụng AWS CLI | 22/04/2026 | 22/04/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - Tìm hiểu EC2 cơ bản: <br>&emsp; + Instance types <br>&emsp; + AMI <br>&emsp; + EBS <br>&emsp; + ... <br> - Các cách remote SSH vào EC2 <br> - Tìm hiểu Elastic IP   <br>                     | 23/04/2026 | 23/04/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **Thực hành:** <br>&emsp; + Tạo EC2 instance <br>&emsp; + Kết nối SSH <br>&emsp; + Gắn EBS volume                                                                                               | 24/04/2026 | 24/04/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 7 - CN | - **Thực hành nâng cao:** <br>&emsp; + Khởi tạo lại EC2 instance và cấu hình Security Group <br>&emsp; + Kết nối SSH và cài đặt Web Server (Apache/Nginx) <br>&emsp; + Triển khai web đơn giản và truy cập qua Public IP <br>&emsp; + Tìm hiểu và thử nghiệm AWS S3 (tạo bucket, upload file) <br>&emsp; + Thực hành thêm các lệnh AWS CLI | 25/04/2026 | 26/04/2026 | <https://cloudjourney.awsstudygroup.com/> |
### Kết quả đạt được tuần 1:

* Hiểu AWS là gì và nắm được các nhóm dịch vụ cơ bản:
  * Compute (EC2)
  * Storage (S3, EBS)
  * Networking (VPC)
  * Database (RDS, DynamoDB)

---

* Đã tạo và cấu hình AWS Free Tier account thành công, sẵn sàng cho việc triển khai và thử nghiệm các dịch vụ.

---

* Làm quen với AWS Management Console:
  * Biết cách tìm kiếm, truy cập và sử dụng các dịch vụ
  * Thực hiện thao tác quản lý tài nguyên thông qua giao diện web

---

* Cài đặt và cấu hình AWS CLI trên máy tính:
  * Thiết lập Access Key, Secret Key
  * Cấu hình region mặc định
  * Kiểm tra kết nối CLI với tài khoản AWS

---

* Sử dụng AWS CLI để thực hiện các thao tác:
  * Kiểm tra thông tin cấu hình (`aws configure list`)
  * Lấy danh sách region (`aws ec2 describe-regions`)
  * Xem thông tin EC2 instance
  * Tạo và quản lý key pair
  * Kiểm tra trạng thái tài nguyên

---

* Đã triển khai thành công EC2 instance:
  * Lựa chọn AMI (Amazon Linux)
  * Cấu hình instance type phù hợp Free Tier
  * Thiết lập Security Group

---

* Đã thực hiện kết nối SSH vào EC2 instance:
  * Sử dụng key pair để truy cập server
  * Thực hiện các lệnh cơ bản trên Linux

---

* Đã cài đặt và chạy thử nghiệm web server (Apache/Nginx) trên EC2:
  * Truy cập thành công thông qua Public IP
  * Kiểm tra hoạt động của server

---

* Đã tạo và gắn EBS volume vào EC2 instance:
  * Thực hiện mount vào hệ thống file
  * Kiểm tra khả năng lưu trữ dữ liệu

---

* (Optional – nếu bạn có làm)
* Đã tạo S3 bucket và upload file thử nghiệm lên hệ thống lưu trữ.

---

* Có khả năng kết hợp sử dụng AWS Management Console và AWS CLI để quản lý tài nguyên AWS một cách linh hoạt và hiệu quả.
