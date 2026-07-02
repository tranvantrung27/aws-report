---
title: "Worklog Tuần 10"
date: 2026-06-22
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

### Mục tiêu tuần 10:

* Hiểu và thực hành thiết lập **VPC Peering** kết nối hai mạng VPC riêng biệt, cấu hình định tuyến và thiết lập **Network ACL (NACL)** để kiểm soát bảo mật cấp độ Subnet.
* Tìm hiểu và triển khai **AWS Transit Gateway (TGW)** để kết nối nhiều VPC theo mô hình Hub-and-Spoke nhằm đơn giản hóa hạ tầng mạng quy mô lớn.
* Thực hành tự động hóa vận hành hệ thống bằng **AWS Lambda** phối hợp với **Amazon EventBridge** để tự động Bật/Tắt các máy chủ EC2 theo khung giờ và gửi thông báo trực tiếp về **Slack**.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Tìm hiểu lý thuyết về VPC Peering, Network ACL (NACL) <br> - **Lab 19 (Phần 1):** Chuẩn bị tài nguyên qua CloudFormation, tạo Security Group và EC2 | 22/06/2026 | 22/06/2026 | [VPC Peering Guide](https://docs.aws.amazon.com/vpc/latest/peering/what-is-vpc-peering.html) <br> [Network ACLs](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html) |
| 3 | - **Lab 19 (Phần 2):** Cấu hình Network ACL, tạo VPC Peering Connection, cấu hình bảng định tuyến và xác minh Cross-Peer DNS | 23/06/2026 | 23/06/2026 | [VPC Peering DNS Support](https://docs.aws.amazon.com/vpc/latest/peering/modify-peering-connections.html) |
| 4 | - Tìm hiểu lý thuyết về AWS Transit Gateway (TGW) <br> - **Lab 20 (Phần 1):** Thiết lập 3 VPC phục vụ demo, tạo Transit Gateway và cấu hình Attachments | 24/06/2026 | 24/06/2026 | [AWS Transit Gateway Guide](https://docs.aws.amazon.com/vpc/latest/tgw/what-is-transit-gateway.html) |
| 5 | - **Lab 20 (Phần 2):** Cấu hình TGW Route Tables và gán các tuyến định tuyến (Routes) trên VPC Route Tables, kiểm tra kết nối chéo giữa các VPC | 25/06/2026 | 25/06/2026 | [TGW Route Tables](https://docs.aws.amazon.com/vpc/latest/tgw/tgw-route-tables.html) |
| 6 | - Tìm hiểu dịch vụ AWS Lambda và các công cụ EventBridge <br> - **Lab 22 (Phần 1):** Tạo Incoming Webhook trên Slack, gắn Tag phân loại EC2 và cấu hình IAM Role cho Lambda | 26/06/2026 | 26/06/2026 | [AWS Lambda Developer Guide](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html) <br> [Slack Webhooks](https://api.slack.com/messaging/webhooks) |
| 7 - CN | - **Lab 22 (Phần 2):** Viết mã nguồn Lambda (Python boto3) Bật/Tắt EC2 và gửi alert về Slack, lên lịch chạy tự động bằng EventBridge, dọn dẹp tài nguyên để tránh phát sinh chi phí | 27/06/2026 | 28/06/2026 | [Boto3 EC2 Client](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/ec2.html) <br> [EventBridge Scheduler](https://docs.aws.amazon.com/scheduler/latest/UserGuide/what-is-scheduler.html) |

### Kết quả đạt được tuần 10:

* **VPC Peering & Security:** Thiết lập thành công liên kết mạng riêng giữa 2 VPC riêng biệt; áp dụng Network ACL để kiểm soát dữ liệu inbound/outbound và hiểu sâu về cơ chế stateless của NACL so với stateful của Security Group.
* **Transit Gateway Deployment:** Thiết lập mạng kết nối chéo Hub-and-Spoke giữa 3 VPC thông qua Transit Gateway, thay thế cho kiến trúc full-mesh truyền thống, giúp dễ dàng mở rộng và quản trị mạng.
* **Serverless Automation:** Triển khai giải pháp tự động hóa bằng AWS Lambda (Python `boto3`) để kiểm tra trạng thái máy chủ dựa trên Resource Tags, thực hiện dừng máy chủ vào cuối giờ làm việc và khởi động lại vào đầu ngày mới.
* **ChatOps Integration:** Tích hợp thành công thông báo thời gian thực từ AWS Lambda về kênh chat Slack thông qua Slack Webhook URL.
* **Scheduled Operations:** Sử dụng Amazon EventBridge Schedule (Cron pattern) để kích hoạt Lambda Functions tự động theo chu kỳ định sẵn.

---

### Chi tiết thực hành Tuần 10:

#### Lab 1: VPC Peering & Network ACL (Lab 19)

##### 1. Giới thiệu (Introduction)
Trong bài thực hành này, chúng ta sẽ tiến hành kết nối hai VPC riêng biệt độc lập (**VPC-A** và **VPC-B**) bằng tính năng **VPC Peering**. Đồng thời, chúng ta sẽ cấu hình **Network ACL (NACL)** - một lá chắn bảo mật dạng stateless ở cấp độ Subnet - để kiểm tra sự khác biệt của cơ chế stateless và cách thiết lập các luật truy cập inbound/outbound để cho phép/chặn traffic.

##### 2. Các bước chuẩn bị (Prerequisites)

###### 2.1 Khởi tạo Hạ tầng bằng CloudFormation
Để nhanh chóng thiết lập hai mạng VPC độc lập kèm theo các subnet và route table cơ bản, ta sử dụng một template AWS CloudFormation. 
* VPC 1 (My VPC): Mạng default VPC `172.31.0.0/16`
* VPC 2 (HG VPC): Mạng custom VPC `10.10.0.0/16`
* Mẫu cấu hình YAML `VPCTemplate.yaml` định nghĩa tài nguyên VPC 2:
```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: 'CloudFormation template to create a custom VPC 2 with CIDR 10.10.0.0/16'
Resources:
  HGVPCLab:
    Type: AWS::EC2::VPC
    Properties:
      CidrBlock: 10.10.0.0/16
      EnableDnsSupport: true
      EnableDnsHostnames: true
      Tags:
        - Key: Name
          Value: HG-VPC-Lab
```
* Minh chứng triển khai thành công Stack CloudFormation tạo VPC 2 trên Console:

![CloudFormation VPC](/images/worklog/week-10/1_cloudformation_create_vpc.png)

###### 2.2 Tạo Security Group
Tạo hai Security Groups tương ứng cho từng VPC:
* `SG-VPC-1`: Cho phép inbound SSH (Port 22) từ IP của bạn và cho phép ICMP (Ping) từ dải CIDR của VPC 2 (`10.10.0.0/16`).
* `SG-VPC-2`: Cho phép inbound SSH (Port 22) từ IP của bạn và cho phép ICMP (Ping) từ dải CIDR của VPC 1 (`172.31.0.0/16`).

###### 2.3 Khởi chạy EC2 Instance
* Khởi chạy máy chủ EC2 trong Public Subnet thuộc VPC 1.
* Khởi chạy máy chủ EC2 trong Public Subnet thuộc VPC 2.

##### 3. Cấu hình VPC Peering
VPC Peering kết nối hai VPC bằng cách định tuyến traffic thông qua dải IP riêng tư.
1. Truy cập **VPC Console** -> **Peering connections** -> **Create peering connection**.
2. Thiết lập thông số:
   * **Name:** `MyVPC-to-HGVPC-Peering`
   * **VPC (Requester):** Chọn `VPC 1` (Default VPC)
   * **VPC (Accepter):** Chọn `VPC 2` (HG-VPC-Lab)
3. Nhấp chọn **Create peering connection**.
4. Trạng thái kết nối sẽ là `Pending acceptance`. Chọn kết nối vừa tạo, nhấp vào **Actions** -> **Accept request**.
5. Minh chứng kết nối VPC Peering hoạt động ở trạng thái `Active`:

![VPC Peering Active](/images/worklog/week-10/2_vpc_peering_active.png)

##### 4. Cấu hình Route Tables (Bảng định tuyến)
Sau khi tạo kết nối Peering, ta cần thêm cấu hình định tuyến để hệ thống biết cách gửi traffic giữa 2 VPC.
* **Tại Route Table của VPC 1:**
  * Thêm dòng Route: Destination `10.10.0.0/16` -> Target: `Peering Connection` (`pcx-09f244ad36ffcd7ba`).
* **Tại Route Table của VPC 2:**
  * Thêm dòng Route: Destination `172.31.0.0/16` -> Target: `Peering Connection` (`pcx-09f244ad36ffcd7ba`).

##### 5. Cấu hình Network ACL (NACL) - Kiểm thử cơ chế Stateless
Khác với Security Group là **Stateful** (chỉ cần cấu hình chiều Inbound là chiều Outbound tự mở), Network ACL hoạt động theo cơ chế **Stateless**. Điều này đồng nghĩa với việc bạn phải cấu hình tường minh cả chiều đi (**Outbound Rules**) lẫn chiều về (**Inbound Rules**).
1. Tạo một Custom NACL và gán liên kết (Associate) nó với Public Subnet của VPC 2.
2. Thiết lập các Inbound Rules và Outbound Rules để cho phép giao tiếp:
   * **Inbound Rules:**
     * Rule 100: Allow SSH (Port 22) từ internet để SSH vào EC2.
     * Rule 110: Allow ICMP (Ping) từ dải IP `172.31.0.0/16` (VPC 1).
     * Rule 120: Allow Ephemeral Ports (Port `1024-65535`) từ bất kỳ đâu.
   * **Outbound Rules:**
     * Rule 100: Allow ICMP (Ping) đi tới dải IP `172.31.0.0/16`.
     * Rule 110: Allow Ephemeral Ports (Port `1024-65535`) đi ra internet.
3. **Thử nghiệm chặn ping:** Đổi Rule 110 của Inbound từ `Allow` sang `Deny`. Thực hiện ping lại từ máy ảo VPC 1 sang máy ảo VPC 2 để kiểm chứng việc chặn traffic thành công.

##### 6. Cấu hình Cross-Peer DNS Support
Theo mặc định, các EC2 Instance nằm chéo VPC Peering sẽ không thể phân giải DNS private của nhau sang IP Private. Ta cần bật tính năng này:

1. Chọn **Peering Connections** -> Chọn `Peering-A-B`.
2. Nhấp chọn **Actions** -> **Edit VPC connection settings**.
3. Tích chọn:
   * **Requester DNS resolution:** `Allow accepter VPC to resolve DNS hostnames to private IPs`
   * **Accepter DNS resolution:** `Allow requester VPC to resolve DNS hostnames to private IPs`
4. Chọn **Save**. Kiểm tra lại bằng cách dùng lệnh `nslookup` hoặc `ping` bằng hostname private của đối phương.

---

##### 7. Dọn dẹp Tài nguyên (Cleanup)
* Xóa hai EC2 Instance.
* Xóa kết nối VPC Peering (Thao tác này tự động gỡ các route liên quan trong Route Tables).
* Xóa CloudFormation Stack để dọn sạch VPC-A và VPC-B.

---

#### Lab 2: AWS Transit Gateway (Lab 20)

##### 1. Giới thiệu (Introduction)
Khi số lượng VPC trong doanh nghiệp tăng lên, việc thiết lập VPC Peering kiểu mạng lưới (full-mesh) sẽ cực kỳ phức tạp và khó kiểm soát. **AWS Transit Gateway (TGW)** hoạt động như một Cloud Router trung tâm kết nối các VPC chéo theo kiến trúc Hub-and-Spoke, giúp quản lý tập trung và định tuyến lưu lượng hiệu quả.

Trong bài lab này, ta sẽ khởi tạo 3 VPC: **VPC-A (10.1.0.0/16)**, **VPC-B (10.2.0.0/16)**, và **VPC-C (10.3.0.0/16)**. Chúng ta sẽ cấu hình để 3 VPC này truyền thông tin chéo qua nhau thông qua một Transit Gateway tập trung.

![Kiến trúc Transit Gateway Hub and Spoke](/images/worklog/week-10/transit-gateway-diagram.png)

##### 2. Các bước chuẩn bị (Preparation)
1. Tạo 3 VPC độc lập có bật tính năng DNS Hostnames:
   * **VPC-A:** CIDR `10.1.0.0/16` - Subnet: `10.1.1.0/24`
   * **VPC-B:** CIDR `10.2.0.0/16` - Subnet: `10.2.1.0/24`
   * **VPC-C:** CIDR `10.3.0.0/16` - Subnet: `10.3.1.0/24`
2. Tạo Security Group cho phép SSH và ICMP (Ping) trên cả 3 VPC.
3. Khởi chạy 1 EC2 Instance trong mỗi VPC tương ứng.

---

##### 3. Tạo Transit Gateway
1. Truy cập **VPC Console** -> **Transit gateways** -> **Create transit gateway**.
2. Điền thông tin:
   * **Name:** `My-Transit-Gateway`
   * **Description:** Transit Gateway connecting VPC-A, B, and C
   * Các tùy chọn khác giữ nguyên mặc định (bao gồm tự động tạo Default Route Table Association và Propagation).
3. Nhấp chọn **Create transit gateway** (Quá trình khởi tạo mất khoảng 3-5 phút, đợi trạng thái chuyển sang `available`).

---

##### 4. Khởi tạo Transit Gateway Attachments
Ta cần liên kết (attach) từng VPC vào Transit Gateway vừa tạo.

1. Chọn **Transit gateway attachments** -> **Create transit gateway attachment**.
2. Cấu hình liên kết cho **VPC-A**:
   * **Transit gateway ID:** Chọn `My-Transit-Gateway`.
   * **Attachment type:** `VPC`
   * **Attachment name:** `Attach-VPC-A`
   * **VPC ID:** Chọn `VPC-A`
   * **Subnet:** Chọn Subnet của VPC-A.
3. Nhấp chọn **Create attachment**.
4. Thực hiện lặp lại bước trên cho **VPC-B** (`Attach-VPC-B`) và **VPC-C** (`Attach-VPC-C`). Đợi tất cả chuyển sang trạng thái `Associated`.

---

##### 5. Cấu hình Định tuyến trên VPC Route Tables
Sau khi liên kết hạ tầng vật lý vào Transit Gateway, ta cần bổ sung route trên các Route Table của từng VPC để chuyển tiếp dữ liệu đến Transit Gateway khi muốn liên lạc chéo VPC.

* **Bảng định tuyến VPC-A:**
  * Thêm Route: Destination `10.2.0.0/16` -> Target: `Transit Gateway` (`tgw-xxxx`)
  * Thêm Route: Destination `10.3.0.0/16` -> Target: `Transit Gateway` (`tgw-xxxx`)
* **Bảng định tuyến VPC-B:**
  * Thêm Route: Destination `10.1.0.0/16` -> Target: `Transit Gateway` (`tgw-xxxx`)
  * Thêm Route: Destination `10.3.0.0/16` -> Target: `Transit Gateway` (`tgw-xxxx`)
* **Bảng định tuyến VPC-C:**
  * Thêm Route: Destination `10.1.0.0/16` -> Target: `Transit Gateway` (`tgw-xxxx`)
  * Thêm Route: Destination `10.2.0.0/16` -> Target: `Transit Gateway` (`tgw-xxxx`)

---

##### 6. Kiểm tra kết quả (Verification)
1. SSH vào **EC2-VPC-A** (`10.1.1.X`).
2. Thực hiện ping sang **EC2-VPC-B** (`10.2.1.Y`) và **EC2-VPC-C** (`10.3.1.Z`):
   ```bash
   ping 10.2.1.Y
   ping 10.3.1.Z
   ```
3. Nếu nhận được tín hiệu trả lời (reply) ổn định, cả 3 mạng VPC đã định tuyến chéo thành công thông qua Transit Gateway.

---

##### 7. Dọn dẹp tài nguyên (Cleanup)
* Xóa các EC2 Instances.
* Truy cập **Transit gateway attachments**, chọn và xóa lần lượt cả 3 Attachments (`Attach-VPC-A`, `Attach-VPC-B`, `Attach-VPC-C`).
* Truy cập **Transit gateways** và chọn xóa `My-Transit-Gateway`.
* Tiến hành xóa các VPC-A, VPC-B, VPC-C để tránh phát sinh hóa đơn.

---

#### Lab 3: Tự động hóa EC2 với AWS Lambda và Thông báo về Slack (Lab 22)

##### 1. Giới thiệu (Introduction)
Tối ưu hóa chi phí (Cost Optimization) là một trụ cột cốt lõi trong AWS Well-Architected Framework. Trong thực tế, các máy chủ môi trường Development/Testing thường không cần chạy 24/7. Bài thực hành này giúp xây dựng quy trình tự động hóa Serverless: sử dụng **AWS Lambda** quét các EC2 Instance có Tag cụ thể để tự động dừng chúng vào lúc 20h00 tối (sau giờ làm việc) và tự động bật lại lúc 08h00 sáng hôm sau, đồng thời gửi thông báo trạng thái trực tiếp về kênh **Slack**.

![Sơ đồ giải pháp Lambda Tự động hóa EC2](/images/worklog/week-10/lambda-ec2-diagram.png)

##### 2. Các bước chuẩn bị (Preparation)

###### 2.1 Chuẩn bị hạ tầng EC2
* Khởi chạy một EC2 Instance chạy hệ điều hành Amazon Linux 2023.
* Gán thẻ tag cho máy chủ:
  * **Key:** `Auto-Stop-Start`
  * **Value:** `true`

###### 2.2 Tạo Slack Incoming Webhook
Để tích hợp gửi tin nhắn từ AWS Lambda về Slack:
1. Truy cập [Slack API Console](https://api.slack.com/apps), bấm chọn **Create New App** -> Chọn **From scratch**.
2. Đặt tên App là `AWS-Ops-Bot` và chọn Workspace của bạn.
3. Trong menu tính năng, chọn **Incoming Webhooks**, bật tính năng sang **On**.
4. Chọn **Add New Webhook to Workspace**, chọn channel nhận tin nhắn (ví dụ: `#aws-alerts`) và nhấn **Allow**.
5. Copy URL Webhook có cấu trúc dạng: `https://hooks.slack.com/services/TXXXXX/BXXXXX/XXXXXXXXXX`.

---

##### 3. Tạo IAM Role cho Lambda Function
Lambda cần được cấp quyền hạn từ IAM Role để có thể thao tác dừng/bật các EC2 instance và tạo Log trên CloudWatch.

1. Truy cập **IAM Console** -> **Roles** -> **Create role**.
2. Chọn **Common use cases** là **Lambda** -> Nhấn **Next**.
3. Chọn **Create policy** dạng JSON và dán nội dung sau:
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ec2:DescribeInstances",
                "ec2:StartInstances",
                "ec2:StopInstances"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "logs:CreateLogGroup",
                "logs:CreateLogStream",
                "logs:PutLogEvents"
            ],
            "Resource": "arn:aws:logs:*:*:*"
        }
    ]
}
```
4. Đặt tên Policy là `Lambda-EC2-Slack-Policy` và đính kèm vào IAM Role. Đặt tên IAM Role là `Lambda-EC2-Slack-Role`.

---

##### 4. Triển khai AWS Lambda Functions
Chúng ta sẽ viết mã nguồn Python sử dụng thư viện SDK `boto3` để tương tác trực tiếp với API của AWS.

###### 4.1 Tạo Function Tự động Dừng Máy chủ (`EC2-Auto-Stop`)
1. Truy cập **AWS Lambda Console** -> **Create function**.
2. Tùy chọn cấu hình:
   * **Function name:** `EC2-Auto-Stop`
   * **Runtime:** `Python 3.12`
   * **Execution role:** Chọn *Use an existing role*, chọn `Lambda-EC2-Slack-Role`.
3. Nhập mã nguồn Python dưới đây vào mục Code:

```python
import json
import urllib.request
import boto3

ec2 = boto3.client('ec2', region_name='ap-southeast-1')
SLACK_WEBHOOK_URL = "https://hooks.slack.com/services/TXXXXX/BXXXXX/XXXXXXXXXX" # Thay bằng URL của bạn

def lambda_handler(event, context):
    # Lọc các EC2 instance đang chạy và có tag Auto-Stop-Start = true
    filters = [
        {'Name': 'tag:Auto-Stop-Start', 'Values': ['true']},
        {'Name': 'instance-state-name', 'Values': ['running']}
    ]
    
    response = ec2.describe_instances(Filters=filters)
    instance_ids = []
    
    for reservation in response['Reservations']:
        for instance in reservation['Instances']:
            instance_ids.append(instance['InstanceId'])
            
    if len(instance_ids) > 0:
        ec2.stop_instances(InstanceIds=instance_ids)
        message = f"🛑 *AWS Lambda Alert:* Đã tự động dừng thành công các EC2 Instances: {', '.join(instance_ids)}"
    else:
        message = "ℹ️ *AWS Lambda Info:* Không tìm thấy EC2 Instance nào đang chạy có tag `Auto-Stop-Start: true` để dừng."
        
    # Gửi thông báo đến Slack
    slack_message = {"text": message}
    req = urllib.request.Request(
        SLACK_WEBHOOK_URL,
        data=json.dumps(slack_message).encode('utf-8'),
        headers={'Content-Type': 'application/json'}
    )
    urllib.request.urlopen(req)
    
    return {
        'statusCode': 200,
        'body': json.dumps(message)
    }
```

4. Nhấn **Deploy** để lưu mã nguồn.

###### 4.2 Tạo Function Tự động Bật Máy chủ (`EC2-Auto-Start`)
1. Tạo function mới tên là `EC2-Auto-Start` với cấu hình tương tự.
2. Dán đoạn mã Python sau:

```python
import json
import urllib.request
import boto3

ec2 = boto3.client('ec2', region_name='ap-southeast-1')
SLACK_WEBHOOK_URL = "https://hooks.slack.com/services/TXXXXX/BXXXXX/XXXXXXXXXX" # Thay bằng URL của bạn

def lambda_handler(event, context):
    # Lọc các EC2 instance đang dừng và có tag Auto-Stop-Start = true
    filters = [
        {'Name': 'tag:Auto-Stop-Start', 'Values': ['true']},
        {'Name': 'instance-state-name', 'Values': ['stopped']}
    ]
    
    response = ec2.describe_instances(Filters=filters)
    instance_ids = []
    
    for reservation in response['Reservations']:
        for instance in reservation['Instances']:
            instance_ids.append(instance['InstanceId'])
            
    if len(instance_ids) > 0:
        ec2.start_instances(InstanceIds=instance_ids)
        message = f"🚀 *AWS Lambda Alert:* Đã tự động khởi động thành công các EC2 Instances: {', '.join(instance_ids)}"
    else:
        message = "ℹ️ *AWS Lambda Info:* Không tìm thấy EC2 Instance nào đang dừng có tag `Auto-Stop-Start: true` để khởi động."
        
    # Gửi thông báo đến Slack
    slack_message = {"text": message}
    req = urllib.request.Request(
        SLACK_WEBHOOK_URL,
        data=json.dumps(slack_message).encode('utf-8'),
        headers={'Content-Type': 'application/json'}
    )
    urllib.request.urlopen(req)
    
    return {
        'statusCode': 200,
        'body': json.dumps(message)
    }
```

3. Nhấn **Deploy** để lưu mã nguồn.

---

##### 5. Cấu hình Trigger tự động bằng Amazon EventBridge Scheduler
Chúng ta sẽ thiết lập lịch trình tự động gọi 2 Lambda Function theo lịch Cron mong muốn.

1. Truy cập giao diện **Amazon EventBridge** -> **Schedulers** -> **Create schedule**.
2. **Cấu hình Schedule dừng máy chủ (Auto-Stop):**
   * **Schedule name:** `Daily-EC2-Stop`
   * **Occurrence:** `Recurring schedule` (Lịch định kỳ)
   * **Schedule type:** `Cron-based schedule`
   * **Expression:** `0 20 ? * MON-FRI *` (Có nghĩa là chạy vào lúc 20:00 tối từ thứ Hai đến thứ Sáu hàng tuần).
   * **Target:** Chọn `AWS Lambda` -> Chọn function `EC2-Auto-Stop`.
   * Nhấp chọn **Create schedule**.
3. **Cấu hình Schedule bật máy chủ (Auto-Start):**
   * **Schedule name:** `Daily-EC2-Start`
   * **Occurrence:** `Recurring schedule`
   * **Schedule type:** `Cron-based schedule`
   * **Expression:** `0 8 ? * MON-FRI *` (Chạy vào lúc 08:00 sáng từ thứ Hai đến thứ Sáu hàng tuần).
   * **Target:** Chọn `AWS Lambda` -> Chọn function `EC2-Auto-Start`.
   * Nhấp chọn **Create schedule**.

---

##### 6. Kiểm tra Kết quả (Check Result)
1. Thử nghiệm thủ công: Vào Lambda Function `EC2-Auto-Stop`, bấm nút **Test** để chạy thử.
2. Kiểm tra giao diện quản lý EC2 Console xem trạng thái của EC2 Instance đã đổi từ `running` sang `stopping`/`stopped` hay chưa.
3. Mở ứng dụng **Slack**, kiểm tra kênh chat đã cấu hình để xác nhận Ops Bot gửi tin nhắn cảnh báo thành công.

![Ops Bot Slack Notification](/images/worklog/week-10/slack-notification.png)

---

##### 7. Dọn dẹp tài nguyên (Clean up resources)
* Xóa 2 Schedule trong Amazon EventBridge Scheduler.
* Xóa 2 Lambda Function (`EC2-Auto-Stop`, `EC2-Auto-Start`).
* Xóa IAM Role `Lambda-EC2-Slack-Role`.
* Xóa EC2 Instance đã tạo để tránh phát sinh chi phí duy trì.
