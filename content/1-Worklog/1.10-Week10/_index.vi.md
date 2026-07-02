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
* Minh chứng thêm route thành công trỏ sang VPC 1 qua liên kết Peering trên bảng định tuyến của VPC 2:

![VPC Route Table Peering](/images/worklog/week-10/4_route_tables_configured.png)

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
3. Minh chứng cấu hình Inbound Rules giới hạn nguồn truy cập từ VPC 1 (`172.31.0.0/16`) trên Network ACL thành công:

![Network ACL Inbound Rules](/images/worklog/week-10/3_nacl_inbound_rules.png)

4. **Thử nghiệm chặn ping:** Đổi Rule 110 của Inbound từ `Allow` sang `Deny`. Thực hiện ping lại từ máy ảo VPC 1 sang máy ảo VPC 2 để kiểm chứng việc chặn traffic thành công.

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

#### Thiết lập AWS Transit Gateway

##### 1. Giới thiệu (Introduction)
Khi số lượng VPC trong doanh nghiệp tăng lên, việc thiết lập VPC Peering kiểu mạng lưới (full-mesh) sẽ cực kỳ phức tạp và khó kiểm soát. **AWS Transit Gateway (TGW)** hoạt động như một Cloud Router trung tâm kết nối các VPC chéo theo kiến trúc Hub-and-Spoke, giúp quản lý tập trung và định tuyến lưu lượng hiệu quả.

Trong bài thực hành này, ta sẽ khởi tạo 4 VPC độc lập chéo nhau:
* **VPC1 (Public):** `172.16.1.0/24`
* **VPC2 (Private):** `172.16.2.0/24`
* **VPC3 (Public):** `172.16.3.0/24`
* **VPC4 (Private):** `172.16.4.0/24`

##### 2. Các bước chuẩn bị (Preparation)
Để nhanh chóng khởi tạo hạ tầng của cả 4 VPC với các dải CIDR không trùng lặp, ta sử dụng một template AWS CloudFormation `tgw-lab.yaml`:
```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: 'Template to create 4 VPCs for AWS Transit Gateway Lab'
Resources:
  VPC1:
    Type: AWS::EC2::VPC
    Properties:
      CidrBlock: 172.16.1.0/24
...
```
* Minh chứng triển khai thành công Stack CloudFormation tạo 4 VPC trên Console:

![CloudFormation TGW VPCs](/images/worklog/week-10/1_cloudformation_tgw_vpc.png)

##### 3. Tạo AWS Transit Gateway
1. Truy cập **VPC Console** -> **Transit gateways** -> **Create transit gateway**.
2. Điền thông tin:
   * **Name:** `lab20-tgw`
   * **Description:** `Transit Gateway for lab20`
   * *Quan trọng:* Bỏ chọn (Uncheck) mục **Default route table association** và **Default route table propagation** để tự cấu hình thủ công.
3. Minh chứng Transit Gateway được tạo thành công ở trạng thái `available`:

![Transit Gateway Created](/images/worklog/week-10/2_tgw_created.png)

##### 4. Khởi tạo Transit Gateway Attachments
Ta cần liên kết (attach) cả 4 VPC vào Transit Gateway vừa tạo.
1. Chọn **Transit gateway attachments** -> **Create transit gateway attachment**.
2. Thực hiện cấu hình liên kết VPC lần lượt cho cả 4 VPC (`VPC1-Attachment`, `VPC2-Attachment`, `VPC3-Attachment`, `VPC4-Attachment`).
3. Minh chứng cả 4 Attachments chuyển sang trạng thái hoạt động `available` thành công:

![TGW Attachments](/images/worklog/week-10/3_tgw_attachments.png)

##### 5. Cấu hình Transit Gateway Route Table (TGW RT)
1. Tạo một Transit Gateway Route Table riêng với tên `lab20-TGW-RT`.
2. **Associations:** Thực hiện liên kết bảng định tuyến này tới cả 4 VPC Attachments.
3. **Propagations:** Thực hiện quảng bá định tuyến của cả 4 VPC Attachments vào bảng định tuyến.
4. Minh chứng Transit Gateway Route Table được liên kết và quảng bá thành công:

![TGW Route Table](/images/worklog/week-10/4_tgw_route_table.png)

##### 6. Cấu hình Định tuyến trên các Route Table của VPC
Bổ sung tuyến đường (Route) trên Route Table của từng VPC để chuyển tiếp dữ liệu đến Transit Gateway:
* **Route Table của VPC 1 và VPC 3:**
  * Thêm Route: Destination `172.16.0.0/16` -> Target: `Transit Gateway` (`lab20-tgw`).
* **Route Table của VPC 2 và VPC 4:**
  * Thêm Route: Destination `0.0.0.0/0` -> Target: `Transit Gateway` (`lab20-tgw`).
* Minh chứng cấu hình Route trỏ dải `172.16.0.0/16` qua Transit Gateway thành công trên Route Table của VPC 1:

![VPC Route to TGW](/images/worklog/week-10/5_vpc_route_to_tgw.png)

##### 7. Dọn dẹp tài nguyên (Cleanup)
Thực hiện dọn dẹp để tránh phát sinh chi phí:
```bash
aws ec2 delete-transit-gateway-vpc-attachment --transit-gateway-attachment-id <attachment-id>
aws ec2 delete-transit-gateway --transit-gateway-id tgw-0a9e3b1327077381a
aws cloudformation delete-stack --stack-name "Lab20-Stack"
```

---

#### Lab 3: Tự động hóa EC2 với AWS Lambda và Thông báo về Slack (Lab 22)

#### Tự động hóa EC2 bằng AWS Lambda và Slack

##### 1. Giới thiệu (Introduction)
Tối ưu hóa chi phí (Cost Optimization) là một trụ cột cốt lõi trong AWS Well-Architected Framework. Trong thực tế, các máy chủ môi trường Development/Testing thường không cần chạy 24/7. Bài thực hành này giúp xây dựng quy trình tự động hóa Serverless: sử dụng **AWS Lambda** quét các EC2 Instance có Tag cụ thể để tự động dừng chúng vào lúc 20h00 tối (sau giờ làm việc) và tự động bật lại lúc 08h00 sáng hôm sau, đồng thời gửi thông báo trạng thái trực tiếp về kênh **Slack**.

##### 2. Các bước chuẩn bị (Preparation)

###### 2.1 Chuẩn bị hạ tầng EC2 & Tags
* Khởi chạy một EC2 Instance chạy hệ điều hành Amazon Linux 2023.
* Gán thẻ tag phân loại cho máy chủ:
  * **Key:** `environment_auto`
  * **Value:** `true`
* Minh chứng cấu hình thẻ tag `environment_auto` cho EC2 instance thành công trên Console:

![EC2 Instance Tags](/images/worklog/week-10/2_ec2_instance_tags.png)

###### 2.2 Tạo Slack Incoming Webhook
Để tích hợp gửi tin nhắn từ AWS Lambda về Slack:
1. Đăng nhập vào Slack và cấu hình tính năng Incoming WebHook.
2. Tạo channel `#notification` nhận tin nhắn.
3. Cài đặt Webhook và sao chép đường dẫn Webhook URL có cấu trúc dạng: `https://hooks.slack.com/services/TXXXXX/BXXXXX/XXXXXXXXXX`.

##### 3. Tạo IAM Role cho Lambda Function
Lambda cần được cấp quyền hạn từ IAM Role để có thể thao tác dừng/bật các EC2 instance và tạo Log trên CloudWatch.
1. Tạo một IAM Role tên `dc-common-lambda-role` với Trusted Entity là `lambda.amazonaws.com`.
2. Đính kèm các Policy có sẵn:
   * `AmazonEC2FullAccess`
   * `CloudWatchFullAccess`
3. Minh chứng IAM Role của Lambda được đính kèm đầy đủ chính sách quyền thành công trên Console:

![IAM Role Lambda](/images/worklog/week-10/1_iam_role_lambda.png)

##### 4. Triển khai AWS Lambda Functions
Viết mã nguồn Python sử dụng thư viện SDK `boto3` để tương tác trực tiếp với API của AWS.

###### 4.1 Tạo Function Tự động Dừng Máy chủ (`dc-common-lambda-auto-stop`)
1. Tạo Lambda Function `dc-common-lambda-auto-stop` chạy môi trường Python 3.11, gán execution role là `dc-common-lambda-role`.
2. Đăng ký biến môi trường (Environment Variable): `environment_auto = true`.
3. Minh chứng cấu hình biến môi trường Environment Variables thành công trên Lambda Console:

![Lambda Environment Variable](/images/worklog/week-10/3_lambda_environment_variable.png)

4. Dán đoạn mã Python sau vào mục Source Code:
```python
import boto3
import os
import json
import urllib3

ec2_resource = boto3.resource('ec2')
http = urllib3.PoolManager()
webhook_url = "https://hooks.slack.com/services/TXXXXX/BXXXXX/XXXXXXXXXX" # Thay bằng URL của bạn

def lambda_handler(event, context):
    environment_auto = os.environ.get('environment_auto')
    if not environment_auto:
        print('Target is empty')
        return {"statusCode": 400, "body": "Target Tag is empty"}
    else:
        instances = ec2_resource.instances.filter(
            Filters=[{'Name': 'tag:environment_auto', 'Values': [environment_auto]}]
        )     
        if not list(instances):
            response = {
                "statusCode": 500,
                "body": "Target Instance is None"
            }
        else:
            action_stop = instances.stop()
            sent_slack(action_stop)
            response = {
                "statusCode": 200,
                "body": "EC2 Stopping"
            }
        return response

def sent_slack(action_stop):
    list_instance_id = []
    if (len(action_stop) > 0) and ("StoppingInstances" in action_stop[0]) and (len(action_stop[0]["StoppingInstances"]) > 0):
        for i in action_stop[0]["StoppingInstances"]:
            list_instance_id.append(i["InstanceId"])
        msg = "Stopping Instances ID:\n %s" % (list_instance_id)
        data = {"text": msg}
        r = http.request("POST",
            webhook_url,
            body = json.dumps(data),
            headers = {"Content-Type": "application/json"})
    else:
        print('Not found Instances Stop')
```

##### 5. Cấu hình Trigger tự động bằng Amazon EventBridge Scheduler
Chúng ta thiết lập lịch trình tự động gọi 2 Lambda Function theo lịch Cron mong muốn để Bật/Tắt EC2 theo giờ:
* Rule Stop (`dc-common-lambda-auto-stop`): Lập lịch tự động tắt máy chủ vào 20:00 tối các ngày trong tuần.
* Rule Start (`dc-common-lambda-auto-start`): Lập lịch tự động bật máy chủ vào 08:00 sáng.
* Minh chứng cấu hình Rule kích hoạt thành công trên Amazon EventBridge Console:

![EventBridge Rule](/images/worklog/week-10/6_eventbridge_rule.png)

##### 6. Kiểm tra Kết quả (Check Result)
1. Thử nghiệm thủ công: Vào Lambda Function `dc-common-lambda-auto-stop`, tạo test event và chạy thử nghiệm (Test).
2. Minh chứng Lambda thực thi thành công trả về `statusCode: 200`:

![Lambda Execution Success](/images/worklog/week-10/4_lambda_execution_success.png)

3. Minh chứng trạng thái EC2 Instance chuyển sang `stopped` thành công sau khi Lambda hoạt động:

![EC2 Stopped Status](/images/worklog/week-10/5_ec2_stopped_status.png)

4. Minh chứng hệ thống AWS CloudWatch Logs ghi nhận lại lịch sử chạy thành công của hàm Lambda:

![CloudWatch Logs](/images/worklog/week-10/7_cloudwatch_logs.png)

##### 7. Dọn dẹp tài nguyên (Clean up resources)
Thực hiện dọn dẹp để tránh phát sinh chi phí:
```bash
aws lambda delete-function --function-name "dc-common-lambda-auto-stop"
aws iam delete-role --role-name "dc-common-lambda-role"
aws ec2 terminate-instances --instance-ids <instance-id>
```
