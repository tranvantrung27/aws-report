---
title: "Worklog Tuần 12"
date: 2026-07-06
weight: 12
chapter: false
pre: " <b> 1.12 </b> "
---

### Mục tiêu tuần 12:

* Tìm hiểu và cấu hình cơ chế kiểm soát truy cập dựa trên thẻ tag (IAM Tag-based Access Control) cho tài nguyên EC2.
* Tìm hiểu công cụ giám sát mã nguồn mở Grafana và cách xây dựng Dashboards cơ bản.
* Thiết lập ranh giới quyền hạn (IAM Permission Boundary) để giới hạn quyền tối đa cho các đối tượng IAM.
* Sử dụng bộ dịch vụ AWS Systems Manager (SSM) quản lý hạ tầng Server từ xa bao gồm Fleet Manager, tự động quét cài đặt bản vá Patch Manager và thực thi lệnh từ xa bằng Run Command.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Tìm hiểu cơ chế IAM Tag-based Access Control <br> - Đọc hướng dẫn và cấu hình IAM Tag để phân quyền kiểm soát truy cập tài nguyên EC2 | 06/07/2026 | 06/07/2026 | [AWS IAM Tagging](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_tags.html) |
| 3 | - Tìm hiểu công cụ giám sát Grafana <br> - Đọc hiểu các khái niệm cơ bản về Grafana và cách thiết lập Dashboard cơ bản | 07/07/2026 | 07/07/2026 | [Grafana User Guide](https://grafana.com/docs/grafana/latest/) |
| 4 | - Tìm hiểu cơ chế IAM Permission Boundary <br> - Cấu hình ranh giới quyền hạn để kiểm soát quyền lực tối đa của các IAM Entity | 08/07/2026 | 08/07/2026 | [AWS Permissions Boundaries](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_boundaries.html) |
| 5 | - Tìm hiểu về AWS Systems Manager (SSM) <br> - Đọc tài liệu giới thiệu SSM, Fleet Manager, Patch Manager và Run Command | 09/07/2026 | 09/07/2026 | [AWS Systems Manager User Guide](https://docs.aws.amazon.com/systems-manager/latest/userguide/what-is-systems-manager.html) |
| 6 | - Cấu hình hạ tầng mạng và IAM Role phục vụ SSM <br> - Tạo VPC `windows-lab-ssm`, Internet Gateway, 2 Subnets <br> - Tạo IAM Role/Instance Profile gắn policy `AmazonSSMManagedInstanceCore` <br> - Khởi chạy 2 máy Windows Server gắn kèm IAM Role | 10/07/2026 | 10/07/2026 | [SSM Agent on Windows Server](https://docs.aws.amazon.com/systems-manager/latest/userguide/agent-install-windows.html) |
| 7 - CN | - Thực hành quản trị bản vá và gửi lệnh từ xa bằng SSM <br> - Sử dụng Patch Manager để quét lỗ hổng và cài đặt OS updates tự động <br> - Sử dụng Run Command thực thi câu lệnh PowerShell `net user` từ xa và lưu log vào S3 <br> - Dọn dẹp toàn bộ tài nguyên tránh phát sinh chi phí | 11/07/2026 | 12/07/2026 | [SSM Patch Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-patch.html) <br> [SSM Run Command](https://docs.aws.amazon.com/systems-manager/latest/userguide/execute-remote-commands.html) |

### Kết quả đạt được tuần 12:

#### 1. Thực hành: Quản lý Patch và thực thi lệnh từ xa trên nhiều Server Windows bằng AWS Systems Manager (SSM)

Trong tuần này, tôi đã thực hành chuỗi Lab thực tế từ **Lab 28 đến Lab 31** về việc triển khai mạng VPC, khởi tạo máy chủ ảo Windows và sử dụng **AWS Systems Manager** để tự động hóa hoạt động vận hành (Fleet Manager, Patch Manager và Run Command):

*   **Tạo hạ tầng mạng VPC công khai**:
    *   Khởi tạo thành công VPC mang tên `windows-lab-ssm` có dải IP `10.10.0.0/16`.
    *   Tạo 2 Subnets công khai ở 2 Availability Zones khác nhau: `windows-lab-ssm-subnet-public1-ap-southeast-1a` (`10.10.1.0/24`) và `windows-lab-ssm-subnet-public2-ap-southeast-1b` (`10.10.2.0/24`).
    *   Tạo Internet Gateway `windows-lab-ssm-igw`, liên kết vào VPC và cấu hình bảng định tuyến Route Table cho phép dải `0.0.0.0/0` đi qua IGW để máy ảo kết nối được internet tải các bản cập nhật.
    *   Bật tính năng tự động gán địa chỉ IP Public (`Auto-assign public IPv4 address`) cho cả 2 Subnets.
    *   *Minh chứng thực tế*:
        *   Tạo thành công VPC: ![VPC Created](/images/worklog/week-12/1_vpc_created_success.png)
        *   Tạo thành công 2 Subnets: ![Subnets Created](/images/worklog/week-12/2_subnets_created_success.png)

*   **Tạo IAM Role cấp quyền quản trị cho Systems Manager**:
    *   Khởi tạo IAM Role mang tên `Windows-Lab-SSM-Role` với Trust Policy cho phép dịch vụ EC2 sử dụng (`ec2.amazonaws.com`).
    *   Gắn policy AWS Managed `AmazonSSMManagedInstanceCore` để kích hoạt giao tiếp bảo mật giữa máy ảo và Systems Manager.
    *   Tạo Instance Profile và gán IAM Role này để chuẩn bị đính kèm vào máy ảo.
    *   *Minh chứng thực tế*:
        *   IAM Role và policy đính kèm thành công: ![IAM Role Created](/images/worklog/week-12/3_iam_role_ssm_attached.png)

*   **Khởi chạy máy chủ Windows Server đính kèm SSM Role**:
    *   Sử dụng AMI Windows Server 2022 mới nhất (`ami-0aa15eda8b6bee073`) khởi tạo 2 máy ảo: `Windows-Lab-SSM-1` (trong Subnet 1a) và `Windows-Lab-SSM-2` (trong Subnet 1b) với instance type hợp lệ `t3.micro`.
    *   Cấu hình gán Instance Profile đại diện cho `Windows-Lab-SSM-Role` cho cả 2 máy ảo.
    *   *Minh chứng thực tế*:
        *   2 máy ảo Windows đang chạy: ![EC2 Instances Running](/images/worklog/week-12/4_ec2_instances_running.png)

*   **Quản lý Nodes bằng Fleet Manager**:
    *   Nhờ có SSM Agent cài sẵn trên Windows Server 2022 và IAM Role được đính kèm đúng cách, Systems Manager đã tự động nhận diện và đưa 2 máy ảo vào danh sách **Managed nodes** ở trạng thái **Online**.
    *   *Minh chứng thực tế*:
        *   Managed nodes hiển thị Online: ![Fleet Manager Managed Nodes](/images/worklog/week-12/5_fleet_manager_managed_nodes.png)

*   **Quét và cập nhật bản vá tự động với Patch Manager**:
    *   Sử dụng công cụ **Patch Manager** cấu hình hoạt động **Scan and install** để quét các lỗ hổng hệ điều hành và cập nhật các bản vá bảo mật theo cấu hình baseline tự động.
    *   Chọn tùy chọn không reboot (`Do not reboot my instances`) để tránh gián đoạn dịch vụ và chỉ định thủ công cả 2 máy ảo Windows.
    *   Quá trình Patch hoàn tất thành công trên cả 2 máy ảo.
    *   *Minh chứng thực tế*:
        *   Cấu hình Patch now: ![Patch Now Config](/images/worklog/week-12/6_patch_now_config.png)
        *   Cài đặt bản vá thành công: ![Patching Succeeded](/images/worklog/week-12/7_patch_manager_succeeded.png)

*   **Gửi câu lệnh PowerShell từ xa bằng Run Command**:
    *   Sử dụng công cụ **Run Command** chạy tài liệu `AWS-RunPowerShellScript` với câu lệnh `net user` để kiểm tra danh sách tài khoản người dùng trên hệ thống từ xa mà không cần RDP vào máy ảo.
    *   Cấu hình lưu trữ đầu ra logs trực tiếp vào S3 Bucket `ssm-runcommand-logs-trantrung04`.
    *   Câu lệnh được thực thi thành công trên cả 2 máy ảo, trả về danh sách tài khoản Windows chi tiết bao gồm `Administrator`, `Guest`, `DefaultAccount`, `WDAGUtilityAccount`.
    *   *Minh chứng thực tế*:
        *   Cấu hình gửi lệnh Run Command: ![Run Command Config](/images/worklog/week-12/8_run_command_config.png)
        *   Trạng thái gửi lệnh Success: ![Run Command Success](/images/worklog/week-12/9_run_command_success_status.png)
        *   Logs đầu ra hiển thị thông tin user: ![Run Command Output](/images/worklog/week-12/10_run_command_output_logs.png)

#### 2. Dọn dẹp tài nguyên sau khi hoàn thành thực hành

Để đảm bảo tối ưu chi phí và không phát sinh bất kỳ khoản phí phát sinh nào ngoài mong muốn trên tài khoản AWS Free Tier, tôi đã dọn dẹp sạch sẽ toàn bộ hạ tầng đã tạo ở tuần này ngay sau khi thực hành xong:
*   Hủy hoàn toàn (**Terminate**) 2 máy ảo Windows Server: `Windows-Lab-SSM-1` và `Windows-Lab-SSM-2`.
*   Xóa bỏ 2 Key Pairs đã tạo: `Windows-Lab-SSM-1-Key` và `Windows-Lab-SSM-2-Key` (đồng thời xóa các file `.pem` lưu cục bộ).
*   Xóa sạch logs và xóa bỏ S3 bucket `ssm-runcommand-logs-trantrung04`.
*   Gỡ bỏ IAM Role khỏi Instance Profile, gỡ bỏ đính kèm policy và xóa IAM Role `Windows-Lab-SSM-Role` cùng Instance Profile.
*   Giải phóng và xóa bỏ hạ tầng mạng bao gồm Subnets, Internet Gateway và VPC `windows-lab-ssm`.



