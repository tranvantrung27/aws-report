---
title: "Worklog Tuần 3"
date: 2026-05-04
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Mục tiêu tuần 3:

* Tìm hiểu dịch vụ Amazon EC2 trên AWS.
* Thực hành khởi tạo, cấu hình và quản lý EC2 Instance.
* Triển khai ứng dụng Node.js trên môi trường Linux và Windows.

### Các công việc cần triển khai trong tuần này:

| Thứ    | Công việc                                                                                                                                                                                                                     | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| ------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2      | - Tìm hiểu tổng quan về Amazon EC2 <br> - Tìm hiểu Instance Types, AMI, Key Pair và Snapshot <br> - Tìm hiểu cơ chế hoạt động của EC2 Instance                                                                            | 04/05/2026   | 04/05/2026      | [Amazon EC2 Basics](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html) <br> [FCJ Lab](https://000004.awsstudygroup.com/) |
| 3      | - Chuẩn bị môi trường EC2 <br>&emsp; + Tạo VPC cho Linux <br>&emsp; + Tạo VPC cho Windows <br>&emsp; + Tạo Security Group cho Linux và Windows Instance                                                                  | 05/05/2026   | 05/05/2026      | [EC2 Security Groups](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-security-groups.html) <br> [FCJ Lab](https://000004.awsstudygroup.com/) |
| 4      | - Khởi tạo Windows EC2 Instance <br>&emsp; + Tạo Windows Instance <br>&emsp; + Kết nối Remote Desktop vào Windows Instance                                                                                                 | 06/05/2026   | 06/05/2026      | [EC2 Windows Guide](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/concepts.html) <br> [FCJ Lab](https://000004.awsstudygroup.com/) |
| 5      | - Khởi tạo Linux EC2 Instance <br>&emsp; + Tạo Linux Instance <br>&emsp; + Kết nối SSH vào Linux Instance <br> - Tìm hiểu thay đổi cấu hình EC2 và quản lý EBS Snapshot                                                 | 07/05/2026   | 07/05/2026      | [EC2 Linux Guide](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html) <br> [FCJ Lab](https://000004.awsstudygroup.com/) |
| 6      | - Thực hành nâng cao EC2 <br>&emsp; + Tạo Custom AMI <br>&emsp; + Tạo Instance từ Custom AMI <br>&emsp; + Thực hành truy cập EC2 khi mất Key Pair                                                                        | 08/05/2026   | 08/05/2026      | [Amazon Machine Images (AMI)](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html) <br> [FCJ Lab](https://000004.awsstudygroup.com/) |
| 7 - CN | - Triển khai ứng dụng Node.js trên EC2 <br>&emsp; + Cài đặt LAMP/XAMPP Server <br>&emsp; + Cài đặt Node.js trên Linux và Windows <br>&emsp; + Deploy ứng dụng Node.js <br> - Tìm hiểu giới hạn tài nguyên bằng IAM | 09/05/2026   | 10/05/2026      | [IAM for Amazon EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-policies-for-amazon-ec2.html) <br> [FCJ Lab](https://000004.awsstudygroup.com/) |

### Kết quả đạt được tuần 3:

#### Về lý thuyết:

* Hiểu rõ khái niệm và cơ chế hoạt động của Amazon EC2 trong AWS.
* Nắm được các thành phần quan trọng của EC2:
  * Instance Types
  * AMI
  * EBS Volume
  * Snapshot
  * Key Pair
  * Security Group
* Hiểu cách quản lý và khởi tạo EC2 Instance trên môi trường Linux và Windows.
* Biết cách tạo Custom AMI và khởi tạo Instance từ AMI tùy chỉnh.
* Hiểu cơ chế phân quyền và giới hạn tài nguyên bằng IAM Policy.

#### Về thực hành:

* EC2 Instance: Đã tạo và cấu hình thành công Linux Instance và Windows Instance trên AWS.
* Kết nối từ xa: Thực hiện thành công SSH vào Linux EC2 và Remote Desktop vào Windows EC2.
* Quản lý lưu trữ: Tạo và quản lý EBS Snapshot, thay đổi cấu hình EC2 và gắn lưu trữ EBS.
* Backup & AMI: Tạo thành công Custom AMI và triển khai Instance mới từ AMI đã tạo.
* Khôi phục truy cập: Thực hành xử lý trường hợp mất Key Pair bằng SSM và User Data.
* Triển khai ứng dụng: Cài đặt thành công LAMP/XAMPP, Node.js và triển khai ứng dụng Node.js trên EC2 Linux và Windows.
* IAM Policy: Thực hành cấu hình giới hạn Region, Instance Type và quyền thao tác tài nguyên AWS.

---

### Chi tiết thực hành Tuần 3:

#### 1. Tổng quan bài Lab
{{% notice info "Information" %}}
Trong bài lab này, chúng ta sẽ triển khai hai Amazon EC2 instance chạy Microsoft Windows Server 2025 và Amazon Linux 2023. Để chuẩn bị cho việc triển khai, chúng ta cần thiết lập môi trường mạng an toàn thông qua Amazon VPC và cấu hình Security Group phù hợp cho từng loại instance.
{{% /notice %}}

{{% notice warning "Security Note" %}}
Việc thiết lập VPC và Security Group riêng biệt cho các instance giúp tăng cường bảo mật và kiểm soát truy cập mạng tốt hơn, đồng thời tuân thủ nguyên tắc phân quyền tối thiểu (principle of least privilege).
{{% /notice %}}

#### 2. Các bước chuẩn bị

##### 2.1 Tạo VPC Linux
{{% notice info "Information" %}}
Amazon Virtual Private Cloud (VPC) cho phép bạn khởi chạy tài nguyên AWS trong một mạng ảo được định nghĩa. Trong phần này, chúng ta sẽ tạo một VPC riêng biệt cho Linux instance với các subnet công khai để có thể truy cập từ internet.
{{% /notice %}}

**Khởi tạo VPC:**
- Trong giao diện **Create VPC**, chọn **VPC and more** để tạo VPC cùng các tài nguyên liên quan.
- Tại **Name tag auto-generation**, nhập **Linux**.
- Màn hình xác nhận tạo thành công:

![Màn hình tạo VPC thành công](/images/worklog/week-3/1.png)

**Cấu hình Subnets:**
- Trong giao diện VPC, chọn **Subnets** từ menu bên trái.
- Ở giao diện cấu hình Subnets:
  - Chọn **Public subnet**.
  - Chọn **Actions**.
  - Chọn **Edit subnet settings**.
- Bật tính năng tự động gán địa chỉ IP công khai:
  - Chọn **Enable auto-assign public IPv4 address**.
  - Chọn **Save**.
- Màn hình xác nhận cấu hình public subnet thành công:

![Cấu hình public subnet thành công](/images/worklog/week-3/2.png)

##### 2.2 Tạo VPC cho Windows Instance
{{% notice info "Information" %}}
Chúng ta sẽ tạo một VPC riêng biệt cho Windows instance tương tự như cách đã làm với Linux instance.
{{% /notice %}}

**Khởi tạo VPC:**
- Truy cập vào **AWS Management Console**, tìm và chọn **VPC**.
- Trong giao diện VPC, chọn **Your VPCs** -> **Create VPC**.
- Chọn **VPC and more**, tại **Name tag auto-generation**, nhập **Windows**.
- Màn hình xác nhận tạo thành công:

![Màn hình tạo VPC Windows thành công](/images/worklog/week-3/3.png)

**Cấu hình Subnets:**
- Thực hiện tương tự như phần 2.1, bật tính năng tự động gán địa chỉ IP công khai cho các Public subnet của VPC Windows.
- Màn hình xác nhận cấu hình thành công:

![Cấu hình public subnet cho Windows thành công](/images/worklog/week-3/4.png)

##### 2.3 Tạo Security Group cho Linux Instance
{{% notice info "Information" %}}
Security Group hoạt động như tường lửa ảo để kiểm soát lưu lượng truy cập. Chúng ta sẽ mở các cổng cần thiết cho Linux (SSH, HTTP, HTTPS, MySQL, Node.js).
{{% /notice %}}

**Thao tác:**
- Chọn **Security Groups** -> **Create security group**.
- **Name:** Linux-SG | **Description:** Security group for Linux instance | **VPC:** Linux-vpc.
- **Inbound rules:** Thêm các rule cho SSH (22), ICMP, HTTP (80), HTTPS (443), MySQL (3306), và Custom TCP (5000).

{{% notice tip "Pro Tip" %}}
Luôn gán Tag cho Security Group để dễ dàng quản lý và phân loại tài nguyên.
{{% /notice %}}

![Hoàn thành tạo Security Group cho Linux instance](/images/worklog/week-3/6.png)

##### 2.4 Tạo Security Group cho Windows Instance
{{% notice info "Information" %}}
Đối với Windows instance, chúng ta cần mở cổng RDP (3389) thay vì SSH để có thể điều khiển từ xa.
{{% /notice %}}

**Thao tác:**
- Thực hiện tương tự như phần 2.3 nhưng chọn VPC Windows.
- **Inbound rules:** Đảm bảo mở cổng **RDP (3389)**, HTTP, HTTPS và ICMP.

![Hoàn thành tạo Security Group cho Windows instance](/images/worklog/week-3/5.png)

{{% notice warning "Warning" %}}
Chỉ nên mở các cổng thực sự cần thiết để giảm thiểu bề mặt tấn công của hệ thống.
{{% /notice %}}

#### 3. Khởi tạo Windows instance

##### 3.1 Tạo Windows Instance
{{% notice info "Information" %}}
Amazon EC2 cung cấp Microsoft Windows Server 2025 như một tùy chọn hệ điều hành cho các workload doanh nghiệp trên AWS Cloud. Hiện tại EC2 đã hỗ trợ AMI Windows Server 2025 kèm giấy phép (License Included).
{{% /notice %}}

{{% notice warning "Warning" %}}
Việc khởi tạo Windows instances có thể phát sinh chi phí. Hãy đảm bảo terminate các instance khi không còn sử dụng để tránh các khoản phí không cần thiết.
{{% /notice %}}

**Các bước thực hiện:**
- Truy cập EC2 Console, chọn **Launch instances**.
- **Name:** Windows-instance.
- **AMI:** Chọn **Windows** -> **Microsoft Windows Server 2025 Base**.
- **Key pair:** Chọn **Create new key pair**.
  - **Key pair name:** kp-windows.
  - **Private key file format:** .pem.
  - Tải về và lưu trữ file `.pem` an toàn.
- **Network settings:** Chọn **Edit**.
  - **VPC:** Windows-vpc.
  - **Subnet:** Public subnet.
  - **Auto-assign public IP:** Enable.
  - **Security groups:** Select existing security group -> **Windows-SG**.
- Kiểm tra lại thông tin và chọn **Launch instance**.

![Khởi tạo instance thành công](/images/worklog/week-3/7.png)

##### 3.2 Kết nối Windows instance
{{% notice info "Information" %}}
Kết nối đến Windows instance trên AWS EC2 được thực hiện thông qua Remote Desktop Protocol (RDP) qua cổng 3389. AWS cung cấp cơ chế bảo mật để lấy mật khẩu quản trị viên bằng cách sử dụng key pair đã tạo trước đó.
{{% /notice %}}

**Các bước thực hiện:**
- Trong giao diện EC2, chọn **Windows-instance** -> **Connect**.
- Chọn tab **RDP Client** -> **Get password**.
- Chọn **Browse** và tải lên file `kp-windows.pem`, sau đó chọn **Decrypt password**.
- Sao chép mật khẩu được hiển thị.
- Tải **Remote desktop file** về máy tính và mở lên.
- Nhập mật khẩu đã giải mã để kết nối.
- Chọn **Yes** khi được hỏi về chứng chỉ bảo mật.

![Màn hình kết nối RDP](/images/worklog/week-3/8.png)

![Hoàn thành kết nối với Microsoft Windows Server 2025 instance](/images/worklog/week-3/9.png)

##### 3.3 Chuẩn bị Sysprep cho custom AMI
{{% notice info "Information" %}}
Sysprep (System Preparation) là công cụ của Microsoft giúp chuẩn bị hệ thống Windows để tạo image. Nó giúp xóa SID và các thông tin đặc thù để có thể nhân bản instance mà không gặp xung đột.
{{% /notice %}}

**Các bước thực hiện:**
- Tại khung tìm kiếm của Windows, nhập và chọn **EC2LaunchSettings**.
- Tại mục **Administrator Password setting**, chọn **Random**.
- Cuối trang, chọn **Shutdown with Sysprep**.
- Xác nhận **Yes** để Windows thực thi Sysprep và tự động tắt máy.

![Cấu hình EC2Launch Settings](/images/worklog/week-3/10.png)

{{% notice warning "Warning" %}}
Nếu không thực hiện Sysprep trước khi tạo AMI, các instance mới tạo từ AMI đó có thể gặp lỗi không thể giải mã mật khẩu (Password Not Available).
{{% /notice %}}

#### 4. Khởi tạo Linux instance

##### 4.1 Tạo Linux instance
{{% notice info "Information" %}}
Amazon Linux 2023 là hệ điều hành Linux do AWS phát triển, được tối ưu hóa đặc biệt cho môi trường điện toán đám mây và cung cấp hiệu suất, bảo mật và tính ổn định cao.
{{% /notice %}}

{{% notice warning "Warning" %}}
Việc khởi tạo EC2 instances có thể phát sinh chi phí trên tài khoản AWS của bạn. Hãy đảm bảo terminate các instance khi không còn sử dụng.
{{% /notice %}}

**Các bước thực hiện:**
- Truy cập EC2 Console, chọn **Launch instances**.
- **Name:** Linux-instance.
- **AMI:** Chọn **Amazon Linux** -> **Amazon Linux 2023 AMI**.
- **Key pair:** Chọn **Create new key pair**.
  - **Key pair name:** kp-linux.
  - **Key pair type:** RSA.
  - **Private key file format:** .pem.
- **Network settings:** Chọn **Edit**.
  - **VPC:** Linux-vpc.
  - **Subnet:** Public subnet.
  - **Auto-assign public IP:** Enable.
  - **Security groups:** Chọn **Linux-SG**.
- Chọn **Launch instance**.

![Hoàn thành khởi tạo Linux instance](/images/worklog/week-3/11.png)

{{% notice tip "Pro Tip" %}}
Status check **"2/2 checks passed"** xác nhận rằng cả kiểm tra hệ thống và kiểm tra instance đều thành công.
{{% /notice %}}

![Trạng thái instance Running](/images/worklog/week-3/12.png)

##### 4.2 Kết nối Linux instance
{{% notice info "Information" %}}
Chúng ta có thể kết nối đến Linux instance thông qua SSH (cổng 22). Trong bài lab này, chúng ta sử dụng **MobaXterm** và **PuTTY**.
{{% /notice %}}

**Cách 1: Sử dụng MobaXterm**
- Mở MobaXterm, chọn **Session** -> **SSH**.
- **Remote host:** Nhập địa chỉ Public IPv4 của instance.
- **Specify username:** ec2-user.
- **Advanced SSH settings:** Chọn **Use private key** và trỏ đến đường dẫn file `kp-linux.pem`.
- Nhấn **OK** và chọn **Accept** ở lần kết nối đầu tiên.

![Giao diện MobaXterm](/images/worklog/week-3/13.png)

Kết nối thành công

![Kết nối thành công qua MobaXterm](/images/worklog/week-3/14.png)

**Cách 2: Sử dụng PuTTY**
{{% notice warning "Security Note" %}}
PuTTY không hỗ trợ trực tiếp định dạng key `.pem` của AWS. Bạn cần sử dụng **PuTTYgen** để chuyển đổi sang định dạng `.ppk`.
{{% /notice %}}

- **Chuyển đổi Key:**
  - Mở PuTTYgen, chọn **Load** -> chọn file `kp-linux.pem`.
  - Chọn **Save private key** và lưu với tên `kp-linux.ppk`.
- **Cấu hình PuTTY:**
  - **Host Name:** Nhập `ec2-user@<Public-IP>`.
  - **Connection** -> **SSH** -> **Auth** -> **Credentials**: Chọn file `.ppk` vừa tạo.
  - Quay lại **Session**, đặt tên và **Save**, sau đó chọn **Open**.

Kết nối thành công

![Kết nối thành công qua PuTTY](/images/worklog/week-3/15.png)

**Kiểm tra kết nối internet:**
- Sử dụng lệnh `ping 8.8.8.8` để kiểm tra.

![Kiểm tra ping thành công](/images/worklog/week-3/16.png)

#### 5. Amazon EC2 cơ bản
{{% notice info "Information" %}}
Bài thực hành này cung cấp một cái nhìn tổng quan khi làm việc với các đối tượng Amazon EC2 và các thành phần liên quan. Chúng ta sẽ tập trung vào những tác vụ cơ bản như thay đổi cấu hình, tạo snapshot, xây dựng AMI tùy chọn, truy cập khi mất key pair.
{{% /notice %}}

{{% notice warning "Security Note" %}}
Trong suốt các bài thực hành này, bạn sẽ học các thực hành bảo mật đúng chuẩn khi quản lý EC2 Instance, bao gồm quản lý key pair an toàn và các phương thức truy cập thay thế trong trường hợp mất thông tin xác thực.
{{% /notice %}}

##### 5.1 Thay đổi cấu hình EC2
{{% notice info "Information" %}}
Việc lựa chọn Amazon EC2 Instance Type phù hợp là một quyết định quan trọng, ảnh hưởng trực tiếp đến năng lực tính toán của máy chủ ảo. Instance Type bạn chọn sẽ tác động đến nhiều yếu tố hiệu năng chính như CPU, RAM, Network và Storage.
{{% /notice %}}

**Các bước thực hiện:**
- Trong giao diện EC2, chọn **Windows-instance**.
- Đảm bảo instance đang ở trạng thái **Stopped**. Nếu chưa, hãy chọn **Instance state** -> **Stop instance** và chờ cho đến khi instance dừng hẳn.
- Chọn **Actions** -> **Instance settings** -> **Change instance type**.
- Tại mục **New instance type**, tìm kiếm và chọn **t3.medium**.
- Chọn **Apply** (hoặc Change).
- Sau khi thay đổi thành công, chọn **Instance state** -> **Start instance**.

![Thay đổi instance type thành công](/images/worklog/week-3/17.png)

{{% notice warning "Warning" %}}
Để thay đổi instance type, bạn phải dừng instance trước. Việc này sẽ làm gián đoạn dịch vụ và mọi ứng dụng đang chạy trên instance. Đảm bảo bạn đã lên kế hoạch cho thời gian ngừng hoạt động này.
{{% /notice %}}

##### 5.2 Tạo và quản lý EBS Snapshots
{{% notice info "Information" %}}
Amazon EBS Snapshot là các bản sao dữ liệu tại một thời điểm cụ thể của volume, được lưu trữ theo cơ chế incremental trên Amazon S3. Snapshot là thành phần quan trọng trong các chiến lược sao lưu dữ liệu và khôi phục thảm họa (disaster recovery).
{{% /notice %}}

**Các bước thực hiện:**
- Trong giao diện EC2, chọn **Snapshots** -> **Create snapshot**.
- Tại giao diện **Create snapshot**:
  - **Resource type:** Chọn **Instance**.
  - **Instance:** Chọn **Windows-instance**.
- Trong phần **Volumes**, bạn có thể chọn **Copy tags from source volume**.
- Chọn **Create snapshot**.
- Đợi khoảng vài phút để trạng thái Snapshot chuyển sang **Completed**.

![Tạo Snapshot thành công](/images/worklog/week-3/18.png)

{{% notice info "Lưu ý" %}}
Khi chọn Resource type là **Instance**, AWS sẽ snapshot tất cả các EBS volume đang được gắn vào EC2 đó. Nếu chọn **Volume**, bạn chỉ snapshot một ổ đĩa cụ thể đã chọn.
{{% /notice %}}

{{% notice tip "Pro Tip" %}}
Sử dụng snapshot để tạo backup định kỳ cho dữ liệu quan trọng. Bạn có thể tự động hóa quá trình này bằng **AWS Backup** hoặc **Amazon Data Lifecycle Manager (DLM)**.
{{% /notice %}}

{{% notice warning "Security Note" %}}
Snapshot chứa toàn bộ dữ liệu của volume, hãy đảm bảo quản lý quyền truy cập snapshot một cách thích hợp để bảo mật thông tin nhạy cảm.
{{% /notice %}}

##### 5.3 Tạo Custom AMI
{{% notice info "Information" %}}
Amazon Machine Image (AMI) là một template chứa cấu hình phần mềm (hệ điều hành, ứng dụng và các cài đặt) cần thiết để khởi chạy một EC2 instance. Tạo Custom AMI cho phép bạn lưu trữ trạng thái hiện tại của một instance để tái sử dụng hoặc triển khai nhiều instance giống nhau.
{{% /notice %}}

**Các bước thực hiện:**
- Trong giao diện EC2, chọn **Windows-instance**.
- Đảm bảo instance đang ở trạng thái **Stopped** (để đảm bảo Sysprep được xử lý hoàn tất).
- Chọn **Actions** -> **Image and templates** -> **Create image**.
- Tại giao diện **Create image**:
  - **Image name:** `Custom Windows AMI`.
  - **Image description:** `Custom Windows AMI`.
  - **No reboot:** Bỏ chọn (hoặc chọn tùy nhu cầu downtime).
- Chọn **Create image**.
- Truy cập vào mục **AMIs** ở menu bên trái để theo dõi quá trình khởi tạo.

![Khởi tạo AMI hoàn tất](/images/worklog/week-3/19.png)

{{% notice warning "Warning" %}}
Trong bài lab này, bạn cần EC2 ở trạng thái hoàn toàn **Stopped** để đảm bảo chức năng Sysprep đã cấu hình trước đó được xử lý hoàn tất trước khi tạo AMI mới.
{{% /notice %}}

{{% notice tip "Pro Tip" %}}
Bỏ chọn tùy chọn **Reboot instance** sẽ ngăn việc tự động reboot EC2 trong quá trình tạo AMI, giúp tránh thay đổi Public IP (nếu không sử dụng Elastic IP).
{{% /notice %}}

{{% notice warning "Security Note" %}}
Custom AMI chứa tất cả dữ liệu và cấu hình. Hãy xóa mọi thông tin nhạy cảm (credentials, private keys) trước khi tạo AMI nếu bạn có ý định chia sẻ nó.
{{% /notice %}}

##### 5.4 Tạo instance từ custom AMI
{{% notice info "Information" %}}
Amazon Machine Images (AMIs) cho phép bạn tạo các EC2 instances mới với cấu hình và phần mềm được cài đặt sẵn. Trong bài lab này, chúng ta sẽ sử dụng custom AMI đã tạo trước đó để khởi chạy một EC2 instance mới.
{{% /notice %}}

**Các bước thực hiện:**
- Trong giao diện EC2, chọn **AMIs**.
- Chọn **Custom Windows AMI** vừa tạo và nhấn **Launch instance from AMI**.
- **Name:** `Windows Server AMI`.
- **Key pair:** Chọn **Create new key pair**.
  - **Key pair name:** `kp-windows2`.
  - **Private key file format:** `.pem`.
- **Network settings:** Chọn **Edit**.
  - **VPC:** Chọn `Windows-vpc`.
  - **Subnet:** Chọn `public subnet`.
  - **Auto-assign public IP:** `Enable`.
  - **Security groups:** Select existing security group -> `Windows-SG`.
- Kiểm tra lại cấu hình và chọn **Launch instance**.

![Hoàn thành tạo instance mới từ AMI](/images/worklog/week-3/20.png)

{{% notice tip "Pro Tip" %}}
Status check **"3/3 checks passed"** xác nhận rằng cả kiểm tra hệ thống, kiểm tra instance và khả năng tiếp cận đều thành công. Instance đã sẵn sàng để kết nối.
{{% /notice %}}

{{% notice warning "Security Note" %}}
Sử dụng key pair mới (`kp-windows2`) để tăng cường bảo mật cho instance mới được tạo từ AMI.
{{% /notice %}}

#### 6. Triển khai ứng dụng Node.js trên Amazon Linux
{{% notice info "Information" %}}
Trong bài lab này, chúng ta sẽ triển khai ứng dụng AWS User Management - một ứng dụng web được xây dựng bằng Node.js, Express, Express-Handlebars và MySQL. Ứng dụng này cho phép thực hiện các thao tác cơ bản với dữ liệu người dùng như xem, thêm, sửa, xóa và tìm kiếm (CRUD).
{{% /notice %}}

{{% notice tip "Pro Tip" %}}
Amazon Linux 2023 là một bản phân phối Linux được AWS tối ưu hóa đặc biệt cho môi trường điện toán đám mây, cung cấp hiệu suất cao và tích hợp tốt với các dịch vụ AWS khác.
{{% /notice %}}

{{% notice warning "Security Note" %}}
Trong môi trường sản xuất thực tế, bạn nên tách riêng cơ sở dữ liệu bằng cách sử dụng **Amazon RDS** thay vì cài đặt MySQL trực tiếp trên EC2 instance để đảm bảo tính sẵn sàng và bảo mật.
{{% /notice %}}

##### 6.1 Cài đặt LAMP web server

###### 6.1.1 Chuẩn bị LAMP Sever
Sau khi thực hiện kết nối vào Amazon Linux 2023 instance, ta tiến hành cài đặt LAMP Stack.

**Các bước thực hiện:**
- Cập nhật hệ thống:
  ```bash
  sudo dnf update -y
  ```
  ![dnf update](/images/worklog/week-3/21.png)

- Cài đặt Apache web server và PHP:
  ```bash
  sudo dnf install -y httpd wget php-fpm php-mysqli php-json php php-devel
  ```
  ![dnf install httpd php](/images/worklog/week-3/22.png)

- Cài đặt MariaDB server:
  ```bash
  sudo dnf install mariadb105-server
  ```
  ![dnf install mariadb](/images/worklog/week-3/23.png)

- Khởi động và cấu hình Apache khởi động cùng hệ thống:
  ```bash
  sudo systemctl start httpd
  sudo systemctl enable httpd
  sudo systemctl is-enabled httpd
  ```
  ![systemctl enable httpd](/images/worklog/week-3/24.png)

- Kiểm tra Apache bằng cách truy cập Public IPv4 trên trình duyệt:
  ![Kiểm tra Apache bằng IP](/images/worklog/week-3/25.png)
  ![Kiểm tra Apache bằng DNS](/images/worklog/week-3/26.png)

- Thực hiện cấp quyền cho thư mục `/var/www`:
  ```bash
  sudo usermod -a -G apache ec2-user
  sudo chown -R ec2-user:apache /var/www
  sudo chmod 2775 /var/www && find /var/www -type d -exec sudo chmod 2775 {} \;
  find /var/www -type f -exec sudo chmod 0664 {} \;
  ```

###### 6.1.2 Kiểm tra LAMP server
Chúng ta thực hiện kiểm tra xem PHP đã hoạt động đúng chưa bằng cách tạo một file `phpinfo.php`.

**Các bước thực hiện:**
- Tạo file PHP test:
  ```bash
  echo "<?php phpinfo(); ?>" > /var/www/html/phpinfo.php
  ```
- Sao chép **Public IPv4 DNS** của instance và dán vào trình duyệt theo cấu trúc: `http://<public-dns>/phpinfo.php`
- Màn hình thông tin PHP hiện ra xác nhận LAMP server hoạt động bình thường:
  ![Kiểm tra phpinfo](/images/worklog/week-3/27.png)

- Xác minh lại các gói đã cài đặt bằng lệnh:
  ```bash
  sudo dnf list installed httpd mariadb-server php-mysqlnd
  ```

- Xóa file `phpinfo.php` để đảm bảo bảo mật:
  ```bash
  rm /var/www/html/phpinfo.php
  ```

###### 6.1.3 Cấu hình database server
Chúng ta sẽ tiến hành bảo mật cho MariaDB server bằng lệnh `mysql_secure_installation`.

**Các bước thực hiện:**
- Khởi động MariaDB:
  ```bash
  sudo systemctl start mariadb
  ```
- Chạy trình cấu hình bảo mật:
  ```bash
  sudo mysql_secure_installation
  ```
- Thực hiện các thiết lập sau:
  - **Enter current password for root:** Nhấn **Enter** (mặc định trống).
  - **Set root password? [Y/n]:** Nhập **Y** và đặt mật khẩu (ví dụ: `123Admin`).
  - **Remove anonymous users? [Y/n]:** Nhập **Y**.
  - **Disallow root login remotely? [Y/n]:** Nhập **Y**.
  - **Remove test database and access to it? [Y/n]:** Nhập **Y**.
  - **Reload privilege tables now? [Y/n]:** Nhập **Y**.

![Cấu hình bảo mật MariaDB](/images/worklog/week-3/28.png)

- Kích hoạt MariaDB khởi động cùng hệ thống:
  ```bash
  sudo systemctl enable mariadb
  ```

{{% notice warning "Security Note" %}}
Hãy ghi lại mật khẩu root này để sử dụng cho việc cấu hình ứng dụng Node.js sau này. Trong môi trường thực tế, nên tạo người dùng riêng cho ứng dụng thay vì dùng tài khoản root.
{{% /notice %}}

###### 6.1.4 Cài đặt phpMyAdmin
phpMyAdmin là một công cụ quản lý cơ sở dữ liệu dựa trên web. Chúng ta sẽ cài đặt nó để quản lý MariaDB dễ dàng hơn.

**Các bước thực hiện:**
- Cài đặt các dependency bắt buộc:
  ```bash
  sudo dnf install php-mbstring php-xml -y
  ```
- Khởi động lại Apache và PHP-FPM để nhận cấu hình mới:
  ```bash
  sudo systemctl restart httpd
  sudo systemctl restart php-fpm
  ```
- Tải và giải nén phpMyAdmin:
  ```bash
  cd /var/www/html
  wget https://www.phpmyadmin.net/downloads/phpMyAdmin-latest-all-languages.tar.gz
  mkdir phpMyAdmin && tar -xvzf phpMyAdmin-latest-all-languages.tar.gz -C phpMyAdmin --strip-components 1
  rm phpMyAdmin-latest-all-languages.tar.gz
  ```
- Truy cập vào đường dẫn: `http://<Public-IPv4-DNS>/phpMyAdmin`
- Đăng nhập với user `root` và mật khẩu đã thiết lập ở bước trước.
  ![Trang đăng nhập phpMyAdmin](/images/worklog/week-3/30.png)

- **Cấu hình Database cho ứng dụng:**
  - Tạo database mới tên là `awsuser`.
  ![Tạo database awsuser](/images/worklog/week-3/31.png)
  - Chọn database `awsuser`, vào mục **SQL** và chạy đoạn lệnh sau để tạo bảng `user`:
    ```sql
    CREATE TABLE `awsuser`.`user` ( 
      `id` INT NOT NULL AUTO_INCREMENT , 
      `first_name` VARCHAR(45) NOT NULL , 
      `last_name` VARCHAR(45) NOT NULL , 
      `email` VARCHAR(45) NOT NULL , 
      `phone` VARCHAR(45) NOT NULL , 
      `comments` TEXT NOT NULL , 
      `status` VARCHAR(10) NOT NULL DEFAULT 'active' , 
      PRIMARY KEY (`id`)
    ) ENGINE = InnoDB;
    ```
  ![Hoàn thành tạo bảng user](/images/worklog/week-3/32.png)

##### 6.2 Cài đặt Node.js trên Linux
{{% notice info "Information" %}}
Node.js là một môi trường runtime JavaScript phía máy chủ, cho phép xây dựng các ứng dụng web có khả năng mở rộng cao. Chúng ta sẽ sử dụng **nvm** để quản lý và cài đặt các phiên bản Node.js.
{{% /notice %}}

{{% notice warning "Warning" %}}
Đảm bảo bạn đã cấu hình Security Group cho EC2 instance để cho phép các kết nối ở cổng **5000** (cổng mặc định của ứng dụng trong bài lab này).
{{% /notice %}}

**Các bước cài đặt:**
- Cài đặt Node Version Manager (nvm):
  ```bash
  curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.0/install.sh | bash
  ```
- Kích hoạt nvm:
  ```bash
  . ~/.nvm/nvm.sh
  ```
- Cài đặt phiên bản Node.js LTS mới nhất:
  ```bash
  nvm install --lts
  ```
- Kiểm tra phiên bản Node.js và npm đã cài đặt thành công:
  ```bash
  node -v
  npm -v
  ```
  ![Cài đặt thành công Node.js](/images/worklog/week-3/33.png)

##### 6.3 Triển khai ứng dụng trên Linux Instance
Chúng ta sẽ tiến hành clone mã nguồn từ GitHub và triển khai ứng dụng thực tế.

**Các bước thực hiện:**
- Cài đặt git:
  ```bash
  sudo dnf install git -y
  ```
- Clone repository code ứng dụng:
  ```bash
  cd ~ec2-user
  git clone https://github.com/First-Cloud-Journey/000004-EC2.git
  cd 000004-EC2
  ```
- Khởi tạo project và cấu hình `package.json`:
  ```bash
  npm init
  ```
  ![Cấu hình npm init](/images/worklog/week-3/34.png)

- Cài đặt các dependencies cần thiết:
  ```bash
  npm install express dotenv express-handlebars body-parser mysql
  ```

{{% notice tip "Khắc phục lỗi bảo mật" %}}
Nếu `npm audit` báo lỗi bảo mật liên quan đến `nodemon`, hãy thử gỡ bỏ và cài đặt lại phiên bản mới nhất:
```bash
npm uninstall --save-dev nodemon
npm install nodemon@latest --save-dev
```
{{% /notice %}}

- Cấu hình database trong file `.env`:
  ```bash
  touch .env
  vi .env
  ```
  Nội dung file `.env`:
  ```env
  DB_HOST=localhost
  DB_NAME=awsuser
  DB_USER=root
  DB_PASS=123Admin
  ```
  ![Kiểm tra cấu hình .env](/images/worklog/week-3/35.png)

- Khởi động ứng dụng:
  ```bash
  npm start
  ```

- Truy cập ứng dụng qua trình duyệt tại cổng **5000** bằng **Public IPv4** hoặc **Public IPv4 DNS** (ví dụ: `http://ec2-13-212-85-21.ap-southeast-1.compute.amazonaws.com:5000`):
  ![Giao diện AWS FCJ Management qua IP](/images/worklog/week-3/36.png)
  ![Giao diện AWS FCJ Management qua DNS](/images/worklog/week-3/37.png)

- **Thực hiện SQL Dummy Data qua phpMyAdmin:**
  Sử dụng đoạn mã `INSERT INTO user ...` để thêm dữ liệu mẫu. Sau khi hoàn tất, refresh lại trang ứng dụng:
  ![Dữ liệu mẫu hiển thị trên ứng dụng](/images/worklog/week-3/38.png)

- **Kiểm tra các chức năng của ứng dụng:**
  - **Xem chi tiết:** ![Xem user](/images/worklog/week-3/39.png)
  - **Chỉnh sửa:** ![Chỉnh sửa user](/images/worklog/week-3/40.png)
  - **Thêm mới:** ![Thêm user](/images/worklog/week-3/41.png)
  - **Tìm kiếm:** ![Tìm kiếm user](/images/worklog/week-3/42.png)
  - **Database thực tế:** ![Database sau insert](/images/worklog/week-3/43.png)
  - **Giao diện console khi chạy server:** ![Giao diện console](/images/worklog/week-3/44.png)
