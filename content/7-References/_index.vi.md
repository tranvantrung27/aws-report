---
title: "Tài liệu tham khảo"
date: 2026-04-26
weight: 7
chapter: false
pre: " <b> 7. </b> "
---

Trong suốt quá trình nghiên cứu, thiết kế kiến trúc và triển khai dự án tốt nghiệp **"Hệ thống Parking IoT thông minh"**, em đã tìm hiểu và tham khảo các tài nguyên tài liệu kỹ thuật, bản vẽ thiết kế phần cứng và hướng dẫn lập trình dưới đây:

---

### 1. Bản vẽ thiết kế sơ đồ mạch (Figma Board)
- **Đường dẫn thiết kế**: [Figma - Thiết kế sơ đồ mạch phần cứng](https://www.figma.com/design/7AJR0OO16WdFWZ6frRpD8a/thi%E1%BA%BFt-k%E1%BA%BF-s%C6%A1-%C4%91%E1%BB%93?node-id=0-1&t=PjFa7iGHmhS23pSQ-1)
  - *Mô tả*: Bản vẽ phác thảo trực quan sơ đồ đi dây mạch điện vật lý, sơ đồ kết nối chân của các mô-đun phần cứng biên (ESP32-CAM và ESP32-S3), tích hợp IC CD74HC4050 và các ngoại vi.

---

### 2. Tài liệu dịch vụ đám mây AWS (AWS Cloud Services)
- **AWS IoT Core Developer Guide**:
  - Hướng dẫn thiết lập kết nối giao thức MQTT bảo mật thông qua chứng chỉ SSL/TLS X.509 cho thiết bị phần cứng nhúng.
  - Link tham khảo: [AWS IoT Core Documentation](https://docs.aws.amazon.com/iot/)
- **AWS Lambda Developer Guide**:
  - Hướng dẫn xây dựng và triển khai hàm xử lý phi máy chủ (Serverless) bằng ngôn ngữ Python (xử lý ảnh) và Node.js (trợ lý ảo AI, API Gateway).
  - Link tham khảo: [AWS Lambda Documentation](https://docs.aws.amazon.com/lambda/)
- **Amazon DynamoDB Developer Guide**:
  - Hướng dẫn thiết kế mô hình dữ liệu NoSQL Single-table, tối ưu hóa khóa phân vùng (Partition Key) và khóa sắp xếp (Sort Key) phục vụ truy vấn thời gian thực.
  - Link tham khảo: [Amazon DynamoDB Documentation](https://docs.aws.amazon.com/dynamodb/)
- **Amazon Rekognition Developer Guide**:
  - Đặc tả API nhận diện vật thể và trích xuất ký tự văn bản (OCR) từ hình ảnh biển số xe chụp bởi ESP32-CAM.
  - Link tham khảo: [Amazon Rekognition Documentation](https://docs.aws.amazon.com/rekognition/)
- **Amazon Bedrock User Guide**:
  - Hướng dẫn tích hợp và gọi mô hình ngôn ngữ lớn (Anthropic Claude 3.5 Sonnet / Claude 3 Haiku) để xây dựng ứng dụng chatbot trợ lý ảo đỗ xe.
  - Link tham khảo: [Amazon Bedrock Documentation](https://docs.aws.amazon.com/bedrock/)

---

### 3. Đặc tả kỹ thuật phần cứng & IoT (Hardware Specifications)
- **ESP32-CAM AI-Thinker Module**:
  - Sơ đồ chân (Pinout Diagram), cấu hình bộ đệm camera OV2640 và quản lý nguồn điện tiết kiệm (Deep Sleep).
  - Link tham khảo: [Espressif ESP32-CAM Specs](https://www.espressif.com/)
- **HC-SR04 Ultrasonic Sensor Datasheet**:
  - Nguyên lý đo khoảng cách bằng sóng siêu âm (phát xung kích hoạt 10 micro giây và tính toán thời gian phản hồi Echo).
  - Link tham khảo: [HC-SR04 Specifications](https://cdn.sparkfun.com/datasheets/Sensors/Proximity/HCSR04.pdf)
- **CD74HC4050 Level Shifter Datasheet**:
  - Đặc tả kỹ thuật của IC chuyển đổi mức logic từ 5V (tín hiệu Echo cảm biến) về mức điện áp an toàn 3.3V cho GPIO của ESP32-S3.
  - Link tham khảo: [Texas Instruments CD74HC4050 Datasheet](https://www.ti.com/product/CD74HC4050)

---

### 4. Tài liệu phát triển phần mềm (Software Development)
- **React & Next.js Documentation**:
  - Hướng dẫn xây dựng ứng dụng Web Single Page App (SPA), cấu hình quản lý trạng thái thời gian thực và tích hợp xác thực bảo mật JWT Token.
  - Link tham khảo: [Next.js Documentation](https://nextjs.org/)
- **AWS Amplify JavaScript SDK**:
  - Thư viện kết nối và đăng nhập tài khoản người dùng thông qua Cognito User Pool và tích hợp GraphQL API trên AppSync.
  - Link tham khảo: [AWS Amplify Docs](https://docs.amplify.aws/)
- **ESP-IDF Programming Guide**:
  - Khung phát triển phần mềm chính thức của Espressif hỗ trợ lập trình Wi-Fi, HTTP Client, điều khiển PWM Servo và quản lý luồng FreeRTOS trên ESP32.
  - Link tham khảo: [ESP-IDF Guide](https://docs.espressif.com/projects/esp-idf/)
