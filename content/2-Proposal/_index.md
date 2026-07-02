---
title: "Proposal"
date: 2026-04-26
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Real-Time IoT Weather Platform on AWS Serverless

> **ITea Lab** — AWS Cloud (Asia Pacific · Singapore) · Infrastructure as Code with AWS CDK

---

## Background & Problem

The *ITea Lab* currently operates multiple physical weather stations, but all data collection and reporting is done manually. As the number of stations grows, centralized management becomes impractical — there is no automated data pipeline, no unified dashboard, and no alerting mechanism when issues arise.

Commercial IoT platforms like **Thingsboard** or **CoreIoT** offer comprehensive features, but are too heavy and expensive for a small research lab with only 5 members.

---

## Our Solution: A Custom AWS Serverless Platform

Instead of relying on a third-party solution, we decided to build our own lightweight, low-cost, and fully automated platform on AWS — tailored to the lab's actual needs, while laying the foundation for a larger system in the future.

The entire infrastructure is defined and deployed using **AWS CDK (Infrastructure as Code)**, ensuring consistency and rapid redeployment capability.

---

## System Architecture

The system supports **5 weather stations** (Raspberry Pi + ESP32), scalable to 15. Sensor data (temperature, humidity, wind, rain) is transmitted via **MQTT** with **X.509 Certificate** authentication to **AWS IoT Core**. From there, an **IoT Core Rule** automatically routes data into the cloud processing pipeline.

*Full system architecture diagram (Asia Pacific · Singapore Region):*

![AWS Serverless Architecture — IoT Weather Platform (ITea Lab)](/images/2-Proposal/IOT.jpg)

**Main data flow:**

```
Sensors (ESP32)
    → Raspberry Pi Edge Device
        → MQTT / X.509 → AWS IoT Core (MQTT Broker)
            → IoT Core Rule → Amazon S3 Raw Data Lake
                → AWS Lambda (Trigger) → AWS Glue Crawler
                    → AWS Glue ETL Job → S3 Processed Analytics Data
                        → API Gateway + Lambda → Next.js Dashboard (Inspira)
                                                    → AWS Amplify + Cognito
```

**AWS Services used:**

| Service | Role in the System |
| :--- | :--- |
| **AWS IoT Core** | MQTT Broker — receives data from 5 stations, secured with X.509 |
| **IoT Core Rule** | Automatically routes raw data into S3 Raw Data Lake |
| **Amazon S3 (x2)** | Raw Data Lake + S3 Processed Analytics Data |
| **S3 Lifecycle Policy + Glacier** | Auto-transitions old data to Glacier for cost-effective long-term storage |
| **AWS Lambda** | Process & Trigger Glue — activates the data processing pipeline |
| **AWS Glue Crawler + ETL Job** | Catalogs and transforms raw data into analytics-ready format |
| **Amazon API Gateway** | REST API that serves processed data to the web dashboard |
| **Next.js + AWS Amplify** | Web dashboard (Inspira template) — real-time data visualization |
| **Amazon Cognito** | User authentication — restricted to 5 Lab Members |
| **CloudWatch + SNS** | Monitors Lambda/Glue logs; alerts on Lambda Errors, Message Count Drop, Budget threshold → Email/SMS to Admin |
| **AWS CDK** | Infrastructure as Code — the entire infrastructure is managed in code |

---

## Deployment Plan

| Phase | Timeline | Activities |
| :--- | :--- | :--- |
| Research & Design | Month 0 (pre-internship) | Hardware study, architecture design, CDK stack writing |
| Estimation & Adjustment | Month 1 | AWS Pricing Calculator, learning relevant services, hardware upgrade |
| Development | Month 2 | Raspberry Pi programming, AWS deployment with CDK, Next.js dashboard build |
| Testing & Launch | Month 3 | End-to-end testing, CloudWatch Alarm configuration, production deployment |
| Data Collection | 1 year post-launch | Continuous operation to accumulate weather data for AI/ML research |

---

## Cost Estimation

Monthly operating costs are extremely low thanks to the Serverless architecture — we only pay for what we use.

| Service | Cost/Month |
| :--- | :--- |
| AWS IoT Core (MQTT) | $0.08 |
| Amazon S3 (x2 buckets) | $0.15 |
| AWS Glue Crawler | $0.07 |
| AWS Glue ETL Jobs | $0.02 |
| AWS Lambda | $0.00 |
| Amazon API Gateway | $0.01 |
| AWS Amplify | $0.35 |
| Data Transfer | $0.02 |
| **Total** | **~$0.70/month · $8.40/year** |

> Hardware ($265 — Raspberry Pi 5 + sensors) is sourced from existing lab equipment.
> Full cost breakdown: [AWS Pricing Calculator](https://calculator.aws/#/estimate?id=621f38b12a1ef026842ba2ddfe46ff936ed4ab01)

---

## Risks & Contingency Plans

| Risk | Mitigation |
| :--- | :--- |
| Network outage at station | Docker local buffer on Raspberry Pi — data is resent once connection is restored |
| ESP32 sensor failure | Regular inspections + spare components on hand |
| AWS cost overrun | CloudWatch Budget Alarm notifies before reaching the threshold |
| Lambda or Glue pipeline error | CloudWatch Alarms + SNS immediate notification to Admin |
| Severe AWS infrastructure failure | Full redeployment from AWS CDK code |

---

## Expected Outcomes

Once complete, the system will:
- **Fully automate** the data pipeline from sensor to dashboard, eliminating manual collection entirely
- **Provide continuous weather data** for 1 year — a valuable resource for the lab's AI/ML research
- **Scale seamlessly** from 5 to 10–15 stations without architectural changes
- **Serve as a reference platform** for building larger IoT systems in the future