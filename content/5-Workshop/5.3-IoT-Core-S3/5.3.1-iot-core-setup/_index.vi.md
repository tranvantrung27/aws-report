---
title: "Cấu hình AWS IoT Core"
date: 2026-04-26
weight: 1
chapter: false
pre: " <b> 5.3.1. </b> "
---

Trong phần này, chúng ta sẽ thực hiện cấu hình **AWS IoT Core** để đăng ký thiết bị phần cứng (ESP32 Camera) và thiết lập các chứng chỉ bảo mật cho phép thiết bị giao tiếp an toàn với đám mây AWS thông qua giao thức MQTT.

---

### Bước 1: Tạo IoT Thing (Đăng ký thiết bị)
1. Đăng nhập vào **AWS Management Console** và tìm kiếm dịch vụ **IoT Core**.
2. Ở thanh điều hướng bên trái, chọn **Manage** -> **All devices** -> **Things**.
3. Nhấp chọn **Create things** để bắt đầu đăng ký thiết bị mới:
   - Chọn **Create single thing** và nhấn **Next**.
   - **Thing name**: Đặt tên thiết bị là `ESP32_Cam_Parking`.
   - Các cấu hình khác giữ nguyên mặc định và nhấn **Next**.

![Đăng ký IoT Thing](/images/5-Workshop/5.3-IoT-Core-S3/5.3.1-create-thing.png)
*(Minh chứng: Ảnh chụp màn hình trang tạo Thing thành công hoặc danh sách Things có chứa ESP32_Cam_Parking)*

---

### Bước 2: Thiết lập Chứng chỉ bảo mật (Certificates)
AWS IoT Core yêu cầu xác thực bằng chứng chỉ X.509 để đảm bảo kết nối bảo mật tuyệt đối:
1. Ở trang **Device certificate**, chọn **Auto-generate new certificate (recommended)**. Nhấn **Next**.
2. **Download các tệp tin chứng chỉ**:
   > [!IMPORTANT]
   > Bạn phải tải toàn bộ các tệp tin này xuống ngay lập tức vì AWS sẽ không cho phép tải lại lần thứ hai sau khi bạn rời trang này.
   - **Device certificate** (dạng `xxx.cert.pem`).
   - **Private key file** (dạng `xxx.private.key`).
   - **Amazon Root CA 1** (chứng chỉ gốc CA của AWS).
3. Lưu các tệp tin này vào một thư mục an toàn trên máy tính của bạn.
4. Nhấn **Next** (khoan nhấn *Create thing* vì chúng ta cần tạo Policy ở bước tiếp theo).

![Tải chứng chỉ bảo mật](/images/5-Workshop/5.3-IoT-Core-S3/5.3.1-certificates.png)
*(Minh chứng: Ảnh chụp màn hình bước tải xuống các file Certificate và Key)*

---

### Bước 3: Tạo và gắn AWS IoT Policy
Policy dùng để phân quyền cụ thể cho thiết bị được phép thực hiện những hành động nào (gửi dữ liệu, nhận tin nhắn, kết nối) trên hệ thống MQTT Broker.

1. Tại trang gắn Policy, nhấp chọn **Create policy** (sẽ mở ra một tab trình duyệt mới).
2. Thiết lập thông số Policy:
   - **Policy name**: Đặt tên là `ESP32_Parking_Policy`.
   - **Policy document**: Chuyển sang chế độ **JSON** và dán đoạn mã phân quyền sau đây vào:
   ```json
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Action": [
           "iot:Connect",
           "iot:Publish",
           "iot:Subscribe",
           "iot:Receive"
         ],
         "Resource": [
           "*"
         ]
       }
     ]
   }
   ```
3. Nhấn **Create** để hoàn thành tạo Policy.
4. Quay lại tab đăng ký Thing trước đó, chọn Policy vừa tạo (`ESP32_Parking_Policy`).
5. Nhấp chọn **Create thing** để hoàn tất quá trình đăng ký thiết bị.

![Cấu hình IoT Policy](/images/5-Workshop/5.3-IoT-Core-S3/5.3.1-iot-policy.png)
*(Minh chứng: Ảnh chụp màn hình cấu hình mã JSON Policy trên AWS Console)*

---

### Bước 4: Lấy thông tin MQTT Endpoint
Mỗi tài khoản AWS sẽ có một MQTT Endpoint riêng biệt để thiết bị kết nối vào:
1. Từ thanh menu bên trái của **AWS IoT Core**, kéo xuống dưới cùng chọn **Settings**.
2. Tại phần **Device data endpoint**, bạn sẽ thấy một chuỗi ký tự dài có cấu trúc dạng:
   `xxxxxxxxxxxxxx-ats.iot.ap-southeast-1.amazonaws.com`
3. Sao chép địa chỉ Endpoint này và lưu lại để cấu hình vào mã nguồn ESP32 sau này.

![MQTT Endpoint](/images/5-Workshop/5.3-IoT-Core-S3/5.3.1-endpoint.png)
*(Minh chứng: Ảnh chụp màn hình phần Settings hiển thị Device data endpoint)*
