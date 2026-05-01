---
title: "Worklog Tuần 2"
date: 2026-04-27
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

### Mục tiêu tuần 2:

* Hiểu kiến trúc mạng trong AWS (VPC).
* Nắm được cách cấu hình các thành phần mạng và bảo mật trong VPC.
* Thực hành triển khai VPC hoàn chỉnh.

### Các công việc cần triển khai trong tuần này:
| Thứ    | Công việc                                                                                                                                                                           | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                                                                                                |
| ------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ------------------------------------------------------------------------------------------------------------- |
| 2      | - Tìm hiểu VPC cơ bản <br>&emsp; + VPC là gì <br>&emsp; + CIDR Block <br>&emsp; + Phân biệt Public / Private Subnet                                                                 | 27/04/2026   | 27/04/2026      | [VPC Basics](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html)                        |
| 3      | - Tìm hiểu Subnets & Route Table <br>&emsp; + Cách chia subnet <br>&emsp; + Cấu hình Route Table                                                                                    | 28/04/2026   | 28/04/2026      | [Subnets & Route Table](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html)                    |
| 4      | - Tìm hiểu Internet Gateway & NAT Gateway <br>&emsp; + Kết nối Internet cho Public Subnet <br>&emsp; + NAT cho Private Subnet                                                       | 29/04/2026   | 29/04/2026      | [NAT Gateways](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html)                         |
| 5      | - Tìm hiểu bảo mật trong VPC <br>&emsp; + Security Group <br>&emsp; + Network ACL <br>&emsp; + Inbound / Outbound rules                                                             | 30/04/2026   | 30/04/2026      | [VPC Security Groups](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html)               |
| 6      | - **Thực hành:** <br>&emsp; + Tạo VPC <br>&emsp; + Tạo Public & Private Subnet <br>&emsp; + Cấu hình Route Table                                                                    | 01/05/2026   | 01/05/2026      | [FCJ Lab - Create VPC](https://cloudjourney.awsstudygroup.com/2-prerequisites/2.1-createvpc/)                 |
| 7 - CN | - **Thực hành nâng cao:** <br>&emsp; + Gắn Internet Gateway <br>&emsp; + Tạo NAT Gateway <br>&emsp; + Deploy EC2 vào Public / Private Subnet <br>&emsp; + Kiểm tra kết nối Internet | 02/05/2026   | 03/05/2026      | [FCJ Lab - Public/Private](https://cloudjourney.awsstudygroup.com/2-prerequisites/2.2-publicprivateinstance/) |


### Kết quả đạt được tuần 2:

**Về lý thuyết:**
* Hiểu rõ kiến trúc và cách hoạt động của Amazon VPC, cách quản lý dải địa chỉ IP qua CIDR.
* Biết cách phân chia Subnet (Public/Private) và cấu hình định tuyến thông qua Route Table.
* Nắm vững các lớp bảo mật Security Group và NACL để bảo vệ tài nguyên trong VPC.

**Về thực hành:**
* **Amazon VPC:** Đã triển khai thành công mô hình mạng VPC tùy chỉnh với đầy đủ các thành phần (IGW, NAT Gateway).
* **Kết nối mạng:** Cấu hình thành công khả năng truy cập Internet bảo mật cho các tài nguyên trong vùng mạng riêng (Private Subnet).
* **Kiểm tra & Triển khai:** Triển khai EC2 Instance và kiểm tra thực tế kết nối, đảm bảo hệ thống mạng hoạt động đúng thiết kế.

---

### Chi tiết thực hành Tuần 2:

#### 1. Khởi tạo Amazon VPC
- **Mô tả:** Tạo một VPC tùy chỉnh với tên **ASG** để thiết lập môi trường mạng cô lập.
- **Thao tác:** 
    - Chọn **VPC only**.
    - Name tag: **ASG**.
    - IPv4 CIDR: **10.10.0.0/16**.
- **Hình ảnh:**
  * Giao diện xác nhận tạo VPC thành công:
![Giao diện tạo VPC thành công](/images/worklog/week-2/1.png)
  * Chi tiết cấu hình thông số VPC ASG:
![Cấu hình thông số VPC](/images/worklog/week-2/2.png)

#### 2. Cấu hình Subnets
- **Mô tả:** Phân chia VPC thành 4 vùng mạng bao gồm 2 Public Subnet và 2 Private Subnet để đảm bảo tính sẵn sàng cao.
- **Thao tác:** 
    - Tạo các Subnet: Public Subnet 1, Public Subnet 2, Private Subnet 1, Private Subnet 2.
    - Gán dải IP tương ứng cho từng Subnet (từ `10.10.1.0/24` đến `10.10.4.0/24`).
- **Hình ảnh:**
  * Danh sách các Subnet (Public/Private) đã được khởi tạo:
![Danh sách Subnet đã tạo](/images/worklog/week-2/3.png)


#### 3. Thiết lập Internet Gateway (IGW)
- **Mô tả:** Internet Gateway là thành phần VPC cho phép kết nối Internet, cung cấp điểm kết nối giữa VPC và Internet, hỗ trợ giao tiếp hai chiều cho các tài nguyên bên trong.
- **Thao tác:** 
    - Truy cập VPC Console, chọn **Internet Gateways**.
    - Tại **Name tag**, nhập **Internet Gateway**.
    - Chọn **Actions** -> **Attach to VPC** để gắn IGW vào VPC **ASG**.
- **Hình ảnh:**
  * Màn hình xác nhận khởi tạo Internet Gateway thành công:
![Màn hình tạo Internet Gateway thành công](/images/worklog/week-2/4.png)
  * Trạng thái Internet Gateway đã được gắn (Attached) vào VPC ASG:
![Gắn Internet Gateway vào VPC thành công](/images/worklog/week-2/5.png)

#### 4. Cấu hình Route Table
- **Mô tả:** Route Table là thành phần định tuyến lưu lượng mạng trong VPC, xác định đường đi cho các gói tin giữa các subnet và Internet.
- **Thao tác:** 
    - Tạo Route Table với tên **Route table-Public**, gán vào VPC **ASG**.
    - Thêm route mới: Destination **0.0.0.0/0** (Internet), Target chọn **Internet Gateway** đã tạo.
    - Thực hiện **Subnet associations** để gắn các Public Subnet vào Route Table này.
- **Hình ảnh:**
  * Màn hình khởi tạo Route Table thành công:
![Màn hình tạo Route Table thành công](/images/worklog/week-2/6.png)
  * Chi tiết cấu hình Route đi Internet (0.0.0.0/0) qua IGW:
![Cấu hình route mới qua Internet Gateway](/images/worklog/week-2/7.png)
  * Xác nhận các Subnet đã được liên kết (Association) vào Route Table:
![Xác nhận cấu hình subnet associations thành công](/images/worklog/week-2/8.png)


#### 5. Thiết lập Security Group (SG)
- **Mô tả:** Security Group hoạt động như một tường lửa ảo cho các instance, kiểm soát lưu lượng vào và ra ở cấp độ instance dựa trên port và protocol.
- **Thao tác:** 
    - **Public Subnet SG:** Tạo SG tên **Public subnet - SG**, cho phép SSH và Ping cho các server trong vùng công khai.
    - **Private Subnet SG:** Tạo SG tên **Private subnet - SG** để quản lý truy cập cho các tài nguyên vùng riêng tư.
    - **VPC Endpoints SG:** Tạo SG tên **VPC-Endpoints-SG**, cấu hình Inbound rule cho phép **HTTPS (port 443)** từ dải mạng VPC (**10.10.0.0/16**).
    - Cập nhật quy tắc Outbound cho Private SG để cho phép HTTPS đi ra các VPC Endpoints.
- **Hình ảnh:**
  * Xác nhận khởi tạo Public Subnet SG thành công:
![Màn hình tạo Public subnet - SG thành công](/images/worklog/week-2/9.png)
  * Xác nhận khởi tạo Private Subnet SG thành công:
![Màn hình tạo Private subnet - SG thành công](/images/worklog/week-2/10.png)
  * Cấu hình Security Group cho VPC Endpoints thành công:
![Cấu hình Security Group cho VPC Endpoints thành công](/images/worklog/week-2/11.png)

#### 6. Kích hoạt VPC Flow Logs
- **Mô tả:** VPC Flow Logs giúp ghi lại thông tin về lưu lượng IP đi vào và ra khỏi các network interface trong VPC, hỗ trợ giám sát bảo mật và khắc phục sự cố mạng.
- **Thao tác:** 
    - Chọn VPC **ASG**, chọn tab **Flow logs** và nhấn **Create flow log**.
    - Cấu hình Filter: **All**, Interval: **1 minute**, Destination: **CloudWatch Logs**.
    - Destination log group: **/aws/vpc/flowlogs**.
    - Tag: Name - **ASG-VPC-FlowLogs**.
- **Hình ảnh:**
  * Xác nhận kích hoạt VPC Flow Logs thành công:
![Kích hoạt VPC Flow Logs thành công](/images/worklog/week-2/12.png)

#### 7. Kiểm tra và Triển khai EC2
- **Mô tả:** Triển khai hạ tầng EC2 thực tế (Public/Private) để kiểm tra khả năng định tuyến, kết nối và bảo mật của toàn bộ hệ thống VPC đã thiết kế theo chuẩn AWS.
- **Thao tác:** 
    - **Tạo EC2 Public:** Đặt tên **EC2 Public**, Instance ID: **i-0fca534a121c6972b**. Gán vào **Public Subnet 1**, bật **Auto-assign public IP**, sử dụng Key pair **aws-keypair** và SG **Public subnet - SG**.
    - **Tạo EC2 Private:** Đặt tên **EC2 Private**, gán vào **Private Subnet**, sử dụng SG **Private subnet - SG**. Ghi lại IP nội bộ (ví dụ: `10.10.4.116`).
    - **Kết nối SSH bằng Visual Studio Code:**
        1. Cài đặt Extension **Remote - SSH** trên VS Code.
        2. Mở file cấu hình SSH (`config`) và thiết lập các thông số: **HostName** (IP Public), **User** (ec2-user) và **IdentityFile** (Đường dẫn tuyệt đối đến file `aws-keypair.pem`).
        3. Chọn Host đã cấu hình và thực hiện kết nối. Xác nhận **Fingerprint** của máy chủ để hoàn tất truy cập.
    - **Kiểm tra trạng thái:** Theo dõi cho đến khi phần **Status check** hiển thị trạng thái **2/2 checks passed** để đảm bảo máy chủ hoạt động bình thường.
- **Hình ảnh:**
  * Xác nhận khởi tạo EC2 Public thành công:
![Xác nhận khởi tạo EC2 Public thành công](/images/worklog/week-2/13.png)
  * Trạng thái máy chủ đã vượt qua các bước kiểm tra (2/2 checks passed):
![Trạng thái kiểm tra 2/2 checks thành công](/images/worklog/week-2/14.png)
  * Xác nhận khởi tạo EC2 Private thành công và ghi nhận thông tin IP nội bộ:
![Xác nhận khởi tạo EC2 Private thành công](/images/worklog/week-2/15.png)
  * Giao diện Terminal xác nhận kết nối SSH thành công vào máy chủ EC2 Public:
![Kết nối SSH thành công vào EC2 Public](/images/worklog/week-2/16.png)

#### 8. Thiết lập NAT Gateway
- **Mô tả:** NAT Gateway cho phép các instances trong private subnet kết nối ra internet, đảm bảo kết nối một chiều từ trong ra ngoài để tăng tính bảo mật cho hệ thống.
- **Thao tác:** 
    - **Tạo Elastic IP Address:** Truy cập EC2 Dashboard, chọn **Elastic IPs** và nhấn **Allocate Elastic IP address** để cấp phát IP tĩnh.
    - **Triển khai NAT Gateway:** Tại VPC Dashboard, chọn **NAT Gateways**, nhấn **Create NAT gateway**. Thiết lập Name: **NAT gateway**, Subnet: **Public subnet 2**, Connectivity type: **Public**, và gán Elastic IP vừa tạo.
    - **Cấu hình Route Table cho Private Subnet:** 
        - Tạo Route table mới tên **Route table - Private**, gán vào **VPC ASG**.
        - **Subnet Associations:** Thực hiện liên kết cả 2 Private Subnets vào Route Table này.
        - **Routes:** Thêm route mới với Destination **0.0.0.0/0** và Target trỏ về **NAT Gateway** đã khởi tạo.
- **Hình ảnh:**
  * Xác nhận tạo thành công Elastic IP:
![Xác nhận tạo thành công Elastic IP](/images/worklog/week-2/16.png)
  * Xác nhận tạo thành công NAT Gateway:
![Xác nhận tạo thành công NAT Gateway](/images/worklog/week-2/17.png)
  * Xác nhận tạo thành công Route Table Private:
![Xác nhận tạo thành công Route Table](/images/worklog/week-2/18.png)
  * Xác nhận liên kết thành công các Private Subnets:
![Thành công liên kết Private Subnets](/images/worklog/week-2/19.png)
  * Xác nhận cấu hình Routes (0.0.0.0/0 trỏ về NAT Gateway):
![Xác nhận cấu hình Routes](/images/worklog/week-2/20.png)

#### 9. Thiết lập EC2 Instance Connect Endpoint (Tùy chọn)
- **Mô tả:** EC2 Instance Connect Endpoint (EIC Endpoint) là giải pháp kết nối an toàn cho phép truy cập các EC2 instances trong Private Subnet mà không cần địa chỉ IP Public hay máy chủ trung gian (Bastion host). Giải pháp này giúp giảm thiểu chi phí quản lý và tăng cường bảo mật cho hệ thống.
- **Thao tác:** 
    - **Tạo Security Group cho EIC Endpoint:** Truy cập Security Groups, nhấn **Create security group**. Cấu hình Name: **EIC Endpoint**, Description: **Allow SSH for MyIP**, và chọn **ASG VPC**.
    - **Triển khai EIC Endpoint:** 
        - Truy cập VPC Dashboard, chọn **Endpoints** và nhấn **Create endpoint**.
        - Thiết lập Name: **EC2 private endpoint**.
        - Service category: Chọn **EC2 Instance Connect Endpoint**.
        - VPC: Chọn **ASG-VPC**.
        - Security group: Chọn **EIC Endpoint** vừa tạo.
        - Subnet: Chọn **Private subnet 2**.
        - Nhấn **Create endpoint** và đợi trạng thái chuyển sang **Available**.

- **Hình ảnh:**
  * Cấu hình tạo EC2 Instance Connect Endpoint thành công:
![Cấu hình EIC Endpoint thành công](/images/worklog/week-2/21.png)

#### 10. Truy cập Shell qua AWS Systems Manager Session Manager
- **Mô tả:** AWS Systems Manager Session Manager cung cấp khả năng truy cập shell tương tác an toàn, dựa trên trình duyệt mà không cần SSH keys, bastion hosts hay mở cổng inbound trong Security Group. Giải pháp này giúp triển khai mô hình bảo mật "Zero Trust" và quản lý truy cập tập trung.
- **Thao tác:** 
    - **Tạo IAM Role:** Khởi tạo role **EC2-SessionManager-Role** với policy **AmazonSSMManagedInstanceCore**.
    - **Gán IAM Role:** Thực hiện **Modify IAM role** cho cả máy chủ EC2 Public và EC2 Private để gán role vừa tạo.
    - **Tạo VPC Endpoints cho SSM:** Thiết lập 3 endpoints cần thiết (**ssm**, **ssmmessages**, **ec2messages**) trong VPC ASG, gắn vào các Private Subnets và sử dụng Security Group dành cho Endpoints.
    - **Sử dụng Session Manager:** Truy cập Systems Manager Console, chọn **Session Manager** và nhấn **Start session** để vào terminal máy chủ trực tiếp trên trình duyệt.
    - **Tối ưu Security Group:** Gỡ bỏ hoàn toàn các quy tắc cho phép SSH (Port 22) để nâng cao tính bảo mật.
- **Hình ảnh:**
  * Khởi tạo IAM Role với policy SSM Core:
![Tạo IAM Role cho SSM](/images/worklog/week-2/22.png)
  * Giao diện kết nối Session Manager thành công vào máy chủ EC2 Private:
![Kết nối Session Manager thành công](/images/worklog/week-2/23.png)

#### 11. Giám sát và Cảnh báo với Amazon CloudWatch
- **Mô tả:** Triển khai giải pháp giám sát toàn diện cho hạ tầng VPC bằng CloudWatch để theo dõi hiệu suất mạng, phát hiện sự cố bảo mật và tối ưu hóa tài nguyên thông qua biểu đồ và cảnh báo tự động qua Email (SNS).
- **Thao tác:** 
    - **Giám sát NAT Gateway:** Thiết lập theo dõi các chỉ số (metrics) then chốt như **BytesInFromDestination**, **PacketDropCount**, và **ActiveConnectionCount**.
    - **Cấu hình CloudWatch Alarms:** Tạo cảnh báo khi số lượng gói tin bị drop (**PacketDropCount**) vượt ngưỡng, đồng thời kết nối với **Amazon SNS** để gửi thông báo tự động.
    - **Phân tích VPC Flow Logs:** Tạo **Metric Filter** để thống kê các kết nối bị từ chối (**REJECT**), giúp nhận diện sớm các nỗ lực xâm nhập trái phép.
    - **Thiết lập Dashboard:** Tạo bảng điều khiển tập trung (**VPC-Monitoring-Dashboard**) để quan sát trực quan toàn bộ trạng thái sức khỏe của hệ thống VPC.
- **Hình ảnh:**
  * Cấu hình CloudWatch Alarm cho NAT Gateway và SNS:
![Cấu hình CloudWatch Alarm](/images/worklog/week-2/24.png)
![VPC Monitoring Dashboard](/images/worklog/week-2/25.png)

#### 12. Cấu hình AWS Site-to-Site VPN (Mạng chi nhánh)
- **Mô tả:** Thiết lập kết nối bảo mật Site-to-Site VPN giữa trụ sở chính (**VPC ASG**) và văn phòng chi nhánh (**VPC ASG VPN**). Mục tiêu là cho phép các EC2 instances ở hai mạng khác nhau có thể giao tiếp an toàn qua địa chỉ IP nội bộ thông qua đường truyền VPN được mã hóa.
- **Thao tác:** 
    - **Khởi tạo VPC Chi nhánh:** Tạo VPC mới mang tên **ASG VPN** với dải IP `10.11.0.0/16`.
    - **Thiết lập Subnet:** Tạo subnet **VPN Public** (`10.11.1.0/24`) và bật tính năng **Auto-assign Public IP**.
    - **Cấu hình Internet:** Tạo và gắn **Internet Gateway VPN** vào VPC chi nhánh để đảm bảo khả năng kết nối ra ngoài cho các thành phần quản trị.
    - **Định tuyến:** Tạo bảng định tuyến **Route table VPN - Public**, cấu hình route `0.0.0.0/0` trỏ về IGW và thực hiện liên kết subnet.
- **Hình ảnh:**
  * Khởi tạo thành công VPC ASG VPN cho mạng chi nhánh:
![Tạo VPC ASG VPN thành công](/images/worklog/week-2/26.png)
  * Cấu hình Subnet VPN Public và bật Auto-assign Public IP:
![Tạo Subnet VPN Public thành công](/images/worklog/week-2/27.png)
  * Xác nhận gắn Internet Gateway vào VPC VPN thành công:
![Attach IGW VPN thành công](/images/worklog/week-2/28.png)
  * Cấu hình Route Table và liên kết Subnet cho mạng VPN:
![Cấu hình Route Table VPN thành công](/images/worklog/week-2/29.png)








