---
title: "Worklog Tuần 5"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Mục tiêu tuần 5:

* Hiểu rõ cơ chế hoạt động của EC2 Auto Scaling: Launch Template, Scaling Policies, Cooldown Period.
* Nắm vững vai trò của Application Load Balancer trong việc phân phối tải và tăng tính sẵn sàng.
* Phân biệt được 4 chiến lược Auto Scaling: Manual, Scheduled, Dynamic, Predictive.
* Hiểu tầm quan trọng của quản lý chi phí AWS và cách dùng AWS Budgets để kiểm soát ngân sách.
* Thực hành triển khai ứng dụng FCJ Management sử dụng Amazon EC2 Auto Scaling và Elastic Load Balancing.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2   | - Tìm hiểu lý thuyết EC2 Auto Scaling: Launch Template, Scaling Policies, Cooldown Period <br> - Nghiên cứu 4 chiến lược Auto Scaling: Manual, Scheduled, Dynamic, Predictive | 18/05/2026 | 18/05/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - Tìm hiểu Application Load Balancer (ALB): cơ chế hoạt động, Target Group, Health Check <br> - Nghiên cứu AWS Budgets và quản lý chi phí AWS | 19/05/2026 | 19/05/2026 | <https://docs.aws.amazon.com/> |
| 4   | - **Thực hành:** <br>&emsp; + Tạo VPC với public/private subnets trải rộng nhiều AZ <br>&emsp; + Cấu hình Auto-assign Public IP cho Public Subnets <br>&emsp; + Tạo Security Group cho ứng dụng FCJ Management (FCJ-Management-SG) <br>&emsp; + Tạo Security Group cho Database Instance (FCJ-Management-DB-SG) | 20/05/2026 | 20/05/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - **Thực hành:** <br>&emsp; + Khởi tạo EC2 instance làm máy chủ web <br>&emsp; + Khởi tạo Database instance với Amazon RDS <br>&emsp; + Cài đặt dữ liệu cho database | 21/05/2026 | 22/05/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **Thực hành:** <br>&emsp; + Triển khai máy chủ web và cấu hình ứng dụng FCJ Management <br>&emsp; + Chuẩn bị metric cho Predictive Scaling | 23/05/2026 | 23/05/2026 | <https://cloudjourney.awsstudygroup.com/> |


### Kết quả đạt được tuần 5:

#### Về lý thuyết:

* Hiểu rõ cơ chế hoạt động của EC2 Auto Scaling: Launch Template, Scaling Policies, Cooldown Period.

* Nắm vững vai trò của Application Load Balancer trong việc phân phối tải và tăng tính sẵn sàng.

* Phân biệt được 4 chiến lược Auto Scaling: Manual, Scheduled, Dynamic, Predictive.

* Hiểu tầm quan trọng của quản lý chi phí AWS và cách dùng AWS Budgets để kiểm soát ngân sách.

#### Về thực hành:

##### 1. Thiết lập hạ tầng mạng (VPC)

Tổng quan bài thực hành: Triển khai ứng dụng **FCJ Management** sử dụng **Amazon EC2 Auto Scaling** cùng với **Elastic Load Balancing**, đảm bảo tính sẵn sàng cao và khả năng mở rộng linh hoạt.

**Các dịch vụ AWS sử dụng:**
* **Amazon VPC**: Tạo môi trường mạng ảo riêng biệt để triển khai các tài nguyên AWS.
* **Amazon EC2**: Cung cấp máy chủ ảo để chạy ứng dụng FCJ Management.
* **Amazon RDS**: Dịch vụ cơ sở dữ liệu quan hệ được quản lý để lưu trữ dữ liệu ứng dụng.
* **Amazon EC2 Auto Scaling**: Tự động điều chỉnh số lượng EC2 instances dựa trên nhu cầu thực tế.
* **Elastic Load Balancing (ELB)**: Phân phối lưu lượng truy cập đến giữa nhiều EC2 instances.
* **Amazon CloudWatch**: Giám sát tài nguyên và ứng dụng, thu thập và theo dõi các metrics.
* **AWS Systems Manager**: Quản lý cấu hình và tự động hóa các tác vụ trên EC2 instances.

**Tạo VPC (AutoScaling-Lab):**

Trong giao diện **Create VPC**, thực hiện cấu hình:
* Chọn **VPC and more**
* **Name**: `AutoScaling-Lab`
* **IPv4 CIDR block**: `10.0.0.0/16`
* Bật tính năng **Auto-assign public IPv4 address** cho Public Subnets

![Cấu hình Auto-assign Public IP cho Public Subnets](/images/worklog/week-5/1.png)

##### 2. Tạo Security Group cho ứng dụng FCJ Management

Thực hiện cấu hình Security Group:
* **Security group name**: `FCJ-Management-SG`
* **Description**: `Security Group for FCJ Management`
* **VPC**: Chọn VPC vừa tạo `AutoScaling-Lab`

![Tạo thành công Security Group FCJ-Management-SG](/images/worklog/week-5/2.png)

##### 3. Tạo Security Group cho Database Instance


Cấu hình Security Group:
* **Security Group name**: `FCJ-Management-DB-SG`
* **Description**: `Security Group for DB instance`
* **VPC**: Chọn VPC vừa tạo
* **Inbound rules**:
  * Protocol: `MYSQL/Aurora` — Port: `3306`
  * Source: `FCJ-Management-SG`

![Tạo thành công Security Group FCJ-Management-DB-SG](/images/worklog/week-5/3.png)

##### Tổng kết phần chuẩn bị hạ tầng:

Đã hoàn thành thiết lập hạ tầng mạng cơ bản bao gồm:
* **VPC và Subnet**: Môi trường mạng ảo riêng biệt với các subnet phân bố ở nhiều Availability Zone
* **Internet Gateway**: Cho phép kết nối từ VPC ra internet
* **Route Table**: Cấu hình định tuyến cho các subnet
* **Security Groups**: Thiết lập các quy tắc bảo mật cho EC2 instances và RDS database

##### 4. Khởi tạo EC2 Instance

**Tạo EC2 Instance**

Truy cập vào AWS Management Console:
1. Tìm và chọn dịch vụ **EC2**.
2. **Cấu hình thông tin cơ bản:** Đặt tên cho instance là `FCJ-Management`.
3. **Chọn Amazon Machine Image (AMI):** Chọn Quick Start -> Amazon Linux -> **Amazon Linux 2023 AMI**.
4. **Chọn loại instance:** Chọn Instance type là `t2.micro`.
5. **Tạo key pair:** Chọn *Create new key pair* -> Đặt tên là `fcj-key`, loại RSA, format `.pem` -> Bấm *Create key pair*.
6. **Cấu hình mạng (Network):** Bấm *Edit*, chọn VPC đã tạo `AutoScaling-Lab`, chọn Public subnet, đảm bảo đã bật Auto-assign public IP.
7. **Cấu hình bảo mật:** Chọn *Select existing security group*, sau đó chọn `FCJ-Management-SG`.
8. Bấm **Launch instance**.

![Khởi tạo instance thành công](/images/worklog/week-5/4.png)

**Kết nối đến EC2 Instance bằng SSH**

Sau khi EC2 instance đã khởi tạo thành công và ở trạng thái “Running”, tiến hành kết nối:

*Đối với người dùng Windows:*
1. Sử dụng **PuTTYgen** để chuyển đổi file `.pem` thành `.ppk`: Mở PuTTYgen -> Load file `fcj-key.pem` -> Chọn *Save private key* và lưu lại.

![Chuyển đổi thành công sang ppk](/images/worklog/week-5/5.png)

2. Mở **PuTTY** và cấu hình:
   * Nhập địa chỉ **Public IP** của EC2 instance vào ô *Host Name*.
   * Trong mục `Connection > SSH > Auth`, chọn *Browse* và tìm đến file `.ppk` vừa tạo.
   * Nhập user là `ec2-user` khi được yêu cầu username.

![Kết nối đến EC2 Instance thành công](/images/worklog/week-5/6.png)

##### 5. Khởi tạo Database Instance với Amazon RDS

**Tạo DB Subnet Group cho Amazon RDS**

Truy cập vào AWS Management Console:
1. Tìm và chọn dịch vụ **RDS**.
2. Chọn **Subnet groups** -> Chọn **Create DB subnet group**.
3. **Cấu hình DB subnet group:**
   * **Name**: Nhập `FCJ-Management-Subnet-Group`.
   * **Description**: Nhập `Subnet Group for FCJ Management`.
   * **VPC**: Chọn VPC đã tạo (`AutoScaling-Lab`).
   * Chọn các Availability Zones và subnets tương ứng.

![Thành công tạo DB Subnet Group với 2 AZ](/images/worklog/week-5/7.png)

**Tạo Amazon RDS Database Instance**

Truy cập vào Amazon RDS Console:
1. Chọn **Databases** -> Chọn **Create database**.
2. **Phương thức tạo database:** Chọn **Standard create**.
3. **Cấu hình Engine:** Chọn **MySQL**.
   4. **Cấu hình Template:** Chọn **Production** -> Chọn **Multi-AZ DB instance**.
   5. **Cài đặt chi tiết (Settings):**
   * **DB instance identifier**: Nhập `fcj-management-db-instance`.
   * **Master username**: Nhập `admin`.
   * Chọn sang **Self managed**.
   * **Master password**: Nhập mật khẩu tùy ý (VD: `123Vodanhphai`) và xác nhận lại.
   6. **Cấu hình Instance:** Chọn `db.m5d.large`, Storage type là `General Purpose SSD (gp3)` với `20 GB` Allocated storage.
7. **Cấu hình kết nối (Connectivity):**
   * Chọn **Don’t connect to an EC2 compute resource**.
   * **VPC**: Chọn `AutoScaling-Lab`.
   * **Subnet group**: Chọn subnet group vừa tạo.
   * **VPC security group**: Chọn **Choose existing** -> Chọn `FCJ-Management-DB-SG`.
   8. **Khởi tạo Database:** Trong phần Additional configuration, nhập Initial database name là `awsfcjuer`.
9. Bấm **Create database**. Quá trình khởi tạo có thể mất 5-10 phút.

Sau khi khởi tạo thành công, ghi nhận lại **Endpoint** và **Port** để cấu hình ứng dụng.

**Tổng kết phần khởi tạo Database:**
* **Tạo DB Subnet Group**: Phân bổ cơ sở dữ liệu trên nhiều Availability Zone.
* **Cấu hình DB Instance**: Lựa chọn loại database, phiên bản, cấu hình phù hợp.
* **Thiết lập bảo mật**: Áp dụng Security Group chuyên biệt kiểm soát truy cập.
* **Khởi tạo cơ sở dữ liệu**: Tạo database ban đầu.

##### 6. Tạo Launch Template

**AMIs và Launch Template**

**Tạo Amazon Machine Images (AMIs) từ EC2**

Ở trong phần giao diện quản lý EC2, ở bảng điều hướng bên trái:
1. Chọn **Instances** -> Chọn instance `FCJ-Management`.
2. Chọn **Actions** -> Chọn **Image and templates** -> Bấm **Create image**.
3. Trong bảng cấu hình *Create AMI*, điền các thông tin sau:
   * **Image name**: `FCJ-Management-AMI`
   * **Image description**: `AMI for FCJ-Management`
4. Bấm **Create Image**.

Sau khi tạo AMI, kiểm tra AMI vừa tạo:
* Chọn **AMIs** trong bảng điều hướng bên trái.
* Tìm và chọn `FCJ-Management-AMI`.

![Tạo AMI thành công](/images/worklog/week-5/8.png)


**Tạo Launch Templates**

Ở giao diện quản lý EC2, ở bảng điều hướng bên trái:
1. Chọn **Launch Templates** -> Chọn **Create launch template**.
2. Trong bảng *Create launch template*, điền các thông tin sau:
   * **Launch template name**: `FCJ-Management-template`
   * **Template version description**: `Template for FCJ Management`
3. Ở phần **Application and OS Image (Amazon Machine Image)**:
   * Chọn tab **My AMIs** -> Chọn **Owned by me**.
   * Chọn AMI đã tạo là `FCJ-Management-AMI`.
4. Ở phần **Instance type**: Chọn `t2.micro`.
5. Ở phần **Key pair (login)**: Chọn Key pair name có tên là `fcj-key`.
6. Ở phần **Network settings**:
   * Chọn subnet public `AutoScaling-Lab-public-ap-southeast-1a`.
   * Chọn **Select existing security group**.
   * Chọn security group `FCJ-Management-SG`.
   7. Cuối cùng, chọn **Create launch template**.

Kiểm tra Launch Template vừa tạo:
![Launch Template tạo thành công](/images/worklog/week-5/9.png)


**Tùy chỉnh nâng cao cho Launch Template**

Ngoài các cấu hình cơ bản, Launch Template còn cho phép bạn tùy chỉnh nhiều thông số nâng cao để tối ưu hóa hiệu suất và chi phí cho các EC2 instance trong Auto Scaling Group.
*Sử dụng User Data*: User Data là một tính năng mạnh mẽ cho phép bạn chạy script khi instance khởi động.

##### 7. Tạo Target Group

**Tạo Target Group**

Ở phần giao diện quản lý EC2, ở bảng điều hướng bên trái, hãy kéo xuống phần **Load Balancing**:
1. Chọn **Target Groups** -> Chọn **Create target group**.
2. Xuất hiện bảng *Specify group details*, hãy cấu hình như sau:
   * Ở phần **Basic configuration**:
     * **Choose a target type**: `Instances`
     * **Target group name**: `FCJ-Management-TG`
     * **Protocol**: `HTTP`
     * **Port**: `5000`
     * **IP address type**: `IPv4`
     * **VPC**: `AutoScaling-Lab`
     * **Protocol version**: `HTTP1`
3. Nhấn **Next**.


**Register targets**:
1. Ở phần **Available instances**:
   * Chọn instance `FCJ-Management`
   * **Ports for the selected instances**: `5000`
   * Chọn **Include as pending below**
2. Ở phần **Review targets**:
   * Kiểm tra target đã được đăng ký.
   * Chọn **Create target group**.


**Kết quả**
Chúng ta đã hoàn thành việc tạo Target Group. Chọn Target Group `FCJ-Management-TG` vừa khởi tạo để xem thông tin chi tiết:

![Chi tiết Target Group](/images/worklog/week-5/10.png)

##### 8. Tạo Load Balancer

**Tạo Application Load Balancer**

Ở giao diện quản lý EC2, ở bảng điều hướng bên trái:
1. Chọn **Load Balancers** -> Nhấn vào nút **Create Load Balancer**.
2. Xuất hiện bảng *Compare and select load balancer type*:
   * Ở phần **Load balancer types**:
   * Tại phần **Application Load Balancer**, chọn **Create**.


Trong bảng *Create Application Load Balancer*:
1. Ở phần **Basic configuration**:
   * **Load balancer name**: `FCJ-Management-LB`
   * **Scheme**: `Internet-facing`
   * **Load balancer IP address type**: `IPv4`
2. Ở phần **Network mapping**:
   * **VPC**: `AutoScaling-Lab`
   * **Subnet**: Chọn Public Subnet ở `ap-southeast-1a`, `ap-southeast-1b`, và `ap-southeast-1c`.
   3. Ở phần **Security groups**:
   * Chọn `FCJ-Management-SG`
4. Ở phần **Listeners and routing**:
   * **Default action**: `FCJ-Management-TG`
   5. Ở phần **Summary**, kiểm tra lại các thông tin đã cấu hình cho Load Balancer.
6. Nhấn vào nút **Create Load Balancer**.

**Xác nhận và kiểm tra Load Balancer**
Sau khi tạo Load Balancer, chọn `FCJ-Management-LB` để xem thông tin chi tiết:

![Tạo Load Balancer thành công](/images/worklog/week-5/11.png)

##### 9. Kiểm tra kết quả

**Kiểm tra kết quả triển khai**

Để truy cập ứng dụng, hãy sử dụng **DNS name** của Load Balancer và dán vào trình duyệt web. 
Kết quả sẽ hiển thị trang web ứng dụng quản lý của chúng ta.


**Kiểm tra tính ổn định của hệ thống**
Để xác minh rằng hệ thống hoạt động ổn định, chúng ta sẽ thực hiện một số thao tác cơ bản. Đầu tiên, hãy thử thay đổi thông tin của một record trong ứng dụng.
Sau khi nhập thông tin, nhấn **Submit** và quan sát thông báo xác nhận:

![Thông báo xác nhận](/images/worklog/week-5/12.png)

Quay lại trang chủ, chúng ta có thể thấy thông tin đã được cập nhật thành công:

![Kết quả sau khi cập nhật](/images/worklog/week-5/13.png)



**Kiểm tra khả năng chịu tải của hệ thống**

Truy cập vào dịch vụ **CloudWatch** từ AWS Management Console. Trong CloudWatch, chúng ta có thể xem các metrics của Load Balancer như RequestCount, TargetResponseTime, và HTTPCode.


**Kiểm tra khả năng tự phục hồi**
Một tính năng quan trọng của kiến trúc sử dụng Load Balancer và Auto Scaling Group là khả năng tự phục hồi khi có instance gặp sự cố. Để kiểm tra tính năng này:
1. Truy cập vào **EC2 Management Console**.
2. Chọn một trong các instance đang chạy trong Auto Scaling Group.
3. Thực hiện thao tác **Terminate instance**.
4. Sau khi terminate instance, Auto Scaling Group sẽ tự động phát hiện và khởi tạo một instance mới để thay thế.


**Kiểm tra khả năng mở rộng tự động**
Auto Scaling Group cho phép hệ thống tự động mở rộng khi tải tăng cao. Để kiểm tra tính năng này, chúng ta có thể tạo tải nhân tạo lên hệ thống và quan sát phản ứng:
* Sử dụng công cụ tạo tải như Apache JMeter hoặc AWS Load Testing.
* Theo dõi các metrics như CPU Utilization trên CloudWatch.
* Quan sát Auto Scaling Group tự động tăng số lượng instance khi tải vượt ngưỡng.


**Kiểm tra khả năng chịu lỗi của Availability Zone**
Một lợi ích lớn của việc triển khai ứng dụng trên nhiều Availability Zone là khả năng chịu lỗi khi một AZ gặp sự cố. Để kiểm tra tính năng này:
1. Truy cập vào **VPC Management Console**.
2. Chọn một subnet trong một AZ cụ thể mà ứng dụng đang chạy.
3. Tạm thời vô hiệu hóa lưu lượng đến subnet đó bằng cách điều chỉnh Network ACLs.
4. Quan sát Load Balancer tự động định tuyến lưu lượng đến các instance trong các AZ còn lại.


##### 10. Tạo Auto Scaling Group

**Vấn đề ở phần trước**

**Thiết lập Auto Scaling Group**

**Tạo Auto Scaling Group**
Ở giao diện quản lý EC2, kéo bảng lựa chọn bên trái xuống cuối:
1. Chọn **Auto Scaling Groups** -> Ấn **Create Auto Scaling group**.
2. Trong giao diện tạo Auto Scaling group, điền các thông tin sau:
   * **Name**: `FCJ-Management-ASG`
   * Trong mục **Launch template**: Chọn `FCJ-Management-template`, Version: `Default (1)`



**Thiết lập mạng**
Trong phần **Network**, cấu hình như sau:
* **VPC**: chọn VPC `AutoScaling-Lab` đã tạo trước đó.
* **Availability Zones and subnets**: chọn 3 public subnets đã tạo.
* Ấn **Next**.

**Thiết lập Load Balancer**

* **Load balancing**: chọn `Attach to an existing load balancer`
* **Attach to an existing load balancer**: chọn `Choose from your load balancer target group`
* **Existing load balancer target group**: chọn `FCJ-Management-TG | HTTP`


* Trong phần **VPC Lattice integration options**: chọn `No VPC Lattice service`.
* Tiếp theo, ở phần **Health checks**: Tích chọn `Turn on Elastic Load Balancing health checks`. Giữ các thiết lập còn lại theo mặc định.
* Trong phần **Additional settings**, ở mục Monitoring: Tích chọn `Enable group metrics collection within CloudWatch`.
* Ấn **Next**.

**Thiết lập Size và Scaling Policies**

* Trong phần **Group size**:
  * **Desired capacity**: `1`
* Trong phần **Scaling limits**:
  * **Min desired capacity**: `1`
  * **Max desired capacity**: `3`
* Trong **Automatic scaling - optional**: chọn `No scaling policies` (tạm thời chưa thiết lập chính sách scaling cho ASG).
* Trong **Instance maintenance policy**: chọn `No policy`.


**Thiết lập thông báo**

Cấu hình thông báo:
* **Send a notification to**: `asg-topic` (tạo topic mới)
* **With these recipients**: nhập email bạn muốn nhận thông báo
* **Event types**: chọn tất cả
* Ấn **Next**.

Xác nhận lại các thông tin và ấn **Create Auto Scaling group**.

**Kết quả**

![Email Subscription](/images/worklog/week-5/14.png)

![Confirm Subscription](/images/worklog/week-5/15.png)

Vì chúng ta đã thiết lập Desired capacity = 1, ASG sẽ tự động tạo một Instance mới, và bạn sẽ nhận được email thông báo:

![Email thông báo tạo Instance](/images/worklog/week-5/16.png)

Vào tab Activity của ASG `FCJ-Management-ASG` để kiểm tra.

##### 11. Cấu hình AWS Budget

**Tổng quan**
Trong phần này, chúng ta tạo AWS Budget sử dụng template có sẵn. AWS Budget là công cụ giúp theo dõi và kiểm soát chi phí AWS một cách hiệu quả.

**Tạo Budget theo template**
Truy cập vào AWS Management Console:
1. Mở AWS Management Console.
2. Tìm và chọn dịch vụ **AWS Billing and Cost Management**.

Trong giao diện AWS Billing and Cost Management:
1. Chọn **Budgets** từ menu bên trái.
2. Nhấn vào **Create a budget**.

Thiết lập cấu hình Budget:
1. Chọn **Use a template (simplified)** để sử dụng mẫu có sẵn.
2. Trong phần Templates, chọn **Monthly cost budget**.

Nhập thông tin chi tiết cho Budget:
1. Đặt tên cho Budget.
2. Xác định số tiền ngân sách hàng tháng.
3. Thiết lập ngưỡng cảnh báo.
4. Nhấn **Create budget** để hoàn tất.

Xác nhận Budget đã được tạo thành công:
![Tạo và kiểm tra Budget thành công](/images/worklog/week-5/17.png)

Xem lịch sử Budget và kiểm tra các loại cảnh báo có sẵn trong template thông qua giao diện Budgets.

##### 12. Tạo Cost Budget (Tùy chỉnh)

**Khởi tạo Cost Budget**
1. Đăng nhập vào AWS Management Console và tìm dịch vụ **Billing and Cost Management**.
2. Tại trang quản trị, chọn **Budgets** từ menu bên trái. Nhấn vào nút **Create budget**.

**Thiết lập cấu hình Budget**
1. Trong phần Budget setup:
   * Chọn **Customize** (tùy chỉnh).
   * Tại Budget types, chọn **Cost budget**.
   * Nhấn **Next**.
2. Trong giao diện Set your budget:
   * Tại **Budget name**, nhập `Monthly`.
   * **Period**: Chọn khoảng thời gian cho Budget là **Monthly**.
   * **Budget effective dates**: Chọn **Recurring Budget** (lặp lại định kỳ).
   * **Specify your monthly budget**: Chọn **Fixed** và nhập số tiền tương ứng vào ô **Budgeted amount**.
3. Tại phần Budget scope, chọn **All AWS services**. Tại Aggregate costs by, chọn **Unblended costs**. Sau đó nhấn **Next**.

**Thiết lập cảnh báo**
1. Chọn **Add an alert threshold** để thiết lập ngưỡng cảnh báo.
2. Thực hiện cấu hình chi tiết cho Alert (như ngưỡng % và email nhận thông báo) và chọn **Next**.
3. Xem lại các thiết lập hành động (nếu có) và chọn **Next**.
4. Xem lại toàn bộ cấu hình và chọn **Create budget** để hoàn tất.

**Xác nhận Budget đã được tạo thành công:**

![Tạo Cost Budget thành công](/images/worklog/week-5/18.png)

##### 13. Tạo Usage Budget

**Khởi tạo Usage Budget**
1. Đăng nhập vào AWS Management Console và tìm dịch vụ **Billing and Cost Management** tại thanh tìm kiếm.
2. Tại trang quản trị, chọn **Budgets** từ menu bên trái. Nhấn vào nút **Create budget**.

**Thiết lập cấu hình Budget**
1. Trong phần Budget setup:
   * Chọn **Customize** (tùy chỉnh)
   * Tại Budget types, chọn **Usage budget**
   * Nhấn **Next**
2. Trong giao diện Set your budget:
   * Tại **Budget name**, nhập tên cho budget của bạn
   * Tại phần **Budget against**:
     * Chọn **Usage type groups**
     * Chọn **EC2: ELB - Running Hours** để theo dõi số giờ chạy của EC2 instances
3. Thực hiện cấu hình Set budget amount:
   * **Period**: Chọn khoảng thời gian cho Budget (Hàng ngày/Hàng tháng/Hàng quý/Hàng năm)
   * **Budget renewal type**: Chọn loại gia hạn ngân sách (Recurring/Expiring)
   * **Budgeting method**: Chọn phương pháp lập ngân sách (Fixed/Planned)
   * Nhập số giờ sử dụng tối đa mà bạn muốn giới hạn
4. Tại phần Budget scope, giữ mặc định và chọn **Next**.

**Thiết lập cảnh báo**
1. Trong phần Configure alerts, chọn **Add an alert threshold** để thiết lập ngưỡng cảnh báo.
2. Hoàn thành thông tin Alert:
   * Thiết lập ngưỡng cảnh báo (% của ngân sách)
   * Thêm địa chỉ email để nhận thông báo khi vượt ngưỡng
3. Nhấn **Next** để tiếp tục.
4. Xem lại các thiết lập và chọn **Create budget** để hoàn tất.

**Xem và quản lý Budget**
Sau khi tạo thành công, bạn sẽ thấy budget mới xuất hiện trong danh sách.

![Xem danh sách Budget](/images/worklog/week-5/19.png)

Bạn có thể kiểm tra **Budget health** (Tình trạng ngân sách) để theo dõi mức sử dụng hiện tại so với ngân sách đã thiết lập, và xem **Budget history** (Lịch sử ngân sách) để theo dõi xu hướng sử dụng qua thời gian.

##### 14. Tạo RI Budget

**Khởi tạo Reservation Budget**
1. Đăng nhập vào trang quản trị AWS Management Console và chọn dịch vụ **AWS Billing and Cost Management** tại thanh tìm kiếm.
2. Tại trang quản trị, chọn **Budgets**. Chọn **Create budget**.

**Thực hiện Budget setup**
1. Chọn **Customize**
2. Chọn **Reservation budget**
3. Chọn **Next**
4. Đặt tên budget.
5. Cấu hình Coverage threshold.
6. Cấu hình Budget scope.
7. Tại phần Alert setting, nhập địa chỉ email. Chọn **Next**.
8. Chọn **Create budget**.

**Hoàn thành tạo budget**
Xác nhận và kiểm tra budget đã tạo thành công trong danh sách.

![Hoàn thành tạo RI Budget](/images/worklog/week-5/20.png)

Sau khi tạo thành công, bạn sẽ thấy thông báo xác nhận. Kiểm tra chi tiết budget đã tạo trong danh sách budgets.

![Chi tiết RI Budget](/images/worklog/week-5/21.png)
