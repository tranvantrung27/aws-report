---
title: "References"
date: 2026-04-26
weight: 7
chapter: false
pre: " <b> 7. </b> "
---

Throughout the research, architectural design, and deployment of the graduate project **"Smart Parking IoT System"**, the following technical documentation, hardware design boards, and programming guides were referenced:

---

### 1. Wiring Diagram Design (Figma Board)
- **Design Link**: [Figma - Hardware Wiring Diagram Design](https://www.figma.com/design/7AJR0OO16WdFWZ6frRpD8a/thi%E1%BA%BFt-k%E1%BA%BF-s%C6%A1-%C4%91%E1%BB%93?node-id=0-1&t=PjFa7iGHmhS23pSQ-1)
  - *Description*: Visual schema mapping the physical wiring layout and pin configurations for the edge hardware modules (ESP32-CAM and ESP32-S3), integrating the CD74HC4050 IC and peripherals.

---

### 2. AWS Cloud Services Documentation
- **AWS IoT Core Developer Guide**:
  - Guide on establishing secure MQTT protocol connections using SSL/TLS X.509 certificates for embedded hardware devices.
  - Reference link: [AWS IoT Core Documentation](https://docs.aws.amazon.com/iot/)
- **AWS Lambda Developer Guide**:
  - Guide on building and deploying Serverless functions using Python (image processing) and Node.js (virtual AI assistant, API Gateway).
  - Reference link: [AWS Lambda Documentation](https://docs.aws.amazon.com/lambda/)
- **Amazon DynamoDB Developer Guide**:
  - Guide on designing NoSQL Single-table data models, optimizing Partition Keys and Sort Keys for real-time querying.
  - Reference link: [Amazon DynamoDB Documentation](https://docs.aws.amazon.com/dynamodb/)
- **Amazon Rekognition Developer Guide**:
  - API specifications for object recognition and text extraction (OCR) from vehicle license plate images captured by the ESP32-CAM.
  - Reference link: [Amazon Rekognition Documentation](https://docs.aws.amazon.com/rekognition/)
- **Amazon Bedrock User Guide**:
  - Guide on integrating and invoking large language models (Anthropic Claude 3.5 Sonnet / Claude 3 Haiku) to build virtual parking assistant chatbot applications.
  - Reference link: [Amazon Bedrock Documentation](https://docs.aws.amazon.com/bedrock/)

---

### 3. Hardware & IoT Technical Specifications
- **ESP32-CAM AI-Thinker Module**:
  - Pinout Diagrams, OV2640 camera buffer configuration, and power management (Deep Sleep).
  - Reference link: [Espressif ESP32-CAM Specs](https://www.espressif.com/)
- **HC-SR04 Ultrasonic Sensor Datasheet**:
  - Working principles of ultrasonic distance measurement (triggering 10 microsecond pulses and calculating Echo duration).
  - Reference link: [HC-SR04 Specifications](https://cdn.sparkfun.com/datasheets/Sensors/Proximity/HCSR04.pdf)
- **CD74HC4050 Level Shifter Datasheet**:
  - Technical specifications of the logic level converter IC converting 5V signals (sensor Echo) to safe 3.3V levels for ESP32-S3 GPIO pins.
  - Reference link: [Texas Instruments CD74HC4050 Datasheet](https://www.ti.com/product/CD74HC4050)

---

### 4. Software Development Resources
- **React & Next.js Documentation**:
  - Guide on building Single Page Applications (SPA), configuring real-time state management, and integrating JWT Token-based authentication.
  - Reference link: [Next.js Documentation](https://nextjs.org/)
- **AWS Amplify JavaScript SDK**:
  - Libraries for authenticating users via Cognito User Pools and integrating GraphQL APIs on AppSync.
  - Reference link: [AWS Amplify Docs](https://docs.amplify.aws/)
- **ESP-IDF Programming Guide**:
  - Official software development framework by Espressif supporting Wi-Fi, HTTP Client, PWM Servo control, and FreeRTOS thread management on ESP32.
  - Reference link: [ESP-IDF Guide](https://docs.espressif.com/projects/esp-idf/)
