---
title: "Week 12 Worklog"
date: 2026-07-06
weight: 12
chapter: false
pre: " <b> 1.12. </b> "
---

### Week 12 Objectives:

* Understand and configure IAM Tag-based Access Control for EC2 resources.
* Learn about open-source monitoring tool Grafana and how to build basic dashboards.
* Set up IAM Permissions Boundaries to delegate permissions and limit the maximum access of IAM Entities.
* Utilize AWS Systems Manager (SSM) suite to manage server infrastructure remotely, including Fleet Manager, automated OS patching via Patch Manager, and executing remote scripts via Run Command.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | - Learn IAM Tag-based Access Control <br> - Read guides and configure IAM Tags for controlling access to EC2 resources | 06/07/2026 | 06/07/2026 | [AWS IAM Tagging](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_tags.html) |
| 3 | - Learn Grafana monitoring basics <br> - Read and understand Grafana concepts and set up a basic Dashboard | 07/07/2026 | 07/07/2026 | [Grafana User Guide](https://grafana.com/docs/grafana/latest/) |
| 4 | - Learn IAM Permission Boundary <br> - Configure permission boundaries to limit the maximum permissions of IAM Entities | 08/07/2026 | 08/07/2026 | [AWS Permissions Boundaries](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_boundaries.html) |
| 5 | - Learn AWS Systems Manager (SSM) concepts <br> - Study introduction to SSM, Fleet Manager, Patch Manager, and Run Command | 09/07/2026 | 09/07/2026 | [AWS Systems Manager User Guide](https://docs.aws.amazon.com/systems-manager/latest/userguide/what-is-systems-manager.html) |
| 6 | - Set up network infrastructure and IAM Roles for SSM <br> - Create VPC `windows-lab-ssm`, Internet Gateway, 2 Subnets <br> - Create IAM Role/Instance Profile with `AmazonSSMManagedInstanceCore` policy <br> - Launch 2 Windows Server EC2 instances attached with the IAM Role | 10/07/2026 | 10/07/2026 | [SSM Agent on Windows Server](https://docs.aws.amazon.com/systems-manager/latest/userguide/agent-install-windows.html) |
| 7 - Sun | - Execute patching and remote commands via SSM <br> - Use Patch Manager to scan vulnerabilities and auto-install OS updates <br> - Use Run Command to run PowerShell `net user` remotely and store logs in S3 <br> - Perform clean-up of all AWS resources to avoid any charges | 11/07/2026 | 12/07/2026 | [SSM Patch Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-patch.html) <br> [SSM Run Command](https://docs.aws.amazon.com/systems-manager/latest/userguide/execute-remote-commands.html) |

### Week 12 Achievements:

#### 1. Practice: Manage Patches and Run Commands on Multiple Windows Servers with AWS Systems Manager (SSM)

This week, I practiced the actual Lab sequence from **Lab 28 to Lab 31** concerning public VPC infrastructure setup, starting Windows virtual instances, and utilizing **AWS Systems Manager** to automate operations (Fleet Manager, Patch Manager, and Run Command):

*   **Create Public VPC Network Infrastructure**:
    *   Successfully created a VPC named `windows-lab-ssm` with CIDR block `10.10.0.0/16`.
    *   Provisioned 2 public subnets in different Availability Zones: `windows-lab-ssm-subnet-public1-ap-southeast-1a` (`10.10.1.0/24`) and `windows-lab-ssm-subnet-public2-ap-southeast-1b` (`10.10.2.0/24`).
    *   Created `windows-lab-ssm-igw` Internet Gateway, attached it to the VPC, and configured the Route Table to direct `0.0.0.0/0` traffic through the IGW.
    *   Enabled the `Auto-assign public IPv4 address` attribute for both subnets.
    *   *Real screenshots*:
        *   Successfully created VPC: ![VPC Created](/images/worklog/week-12/1_vpc_created_success.png)
        *   Successfully created Subnets: ![Subnets Created](/images/worklog/week-12/2_subnets_created_success.png)

*   **Create IAM Role for Systems Manager Management**:
    *   Created an IAM Role named `Windows-Lab-SSM-Role` with an EC2 Trust Relationship (`ec2.amazonaws.com`).
    *   Attached the `AmazonSSMManagedInstanceCore` AWS Managed Policy to establish secure communication between the instances and AWS Systems Manager.
    *   Created an Instance Profile containing this role to attach to target instances.
    *   *Real screenshots*:
        *   IAM Role and policy attached successfully: ![IAM Role Created](/images/worklog/week-12/3_iam_role_ssm_attached.png)

*   **Launch Windows Server Instances attached with SSM Role**:
    *   Launched 2 Windows Server 2022 instances (`ami-0aa15eda8b6bee073`) named `Windows-Lab-SSM-1` (Subnet 1a) and `Windows-Lab-SSM-2` (Subnet 1b) using the eligible `t3.micro` instance type.
    *   Associated the `Windows-Lab-SSM-Instance-Profile` with both instances during launch.
    *   *Real screenshots*:
        *   2 Windows EC2 instances running: ![EC2 Instances Running](/images/worklog/week-12/4_ec2_instances_running.png)

*   **Manage Nodes via Fleet Manager**:
    *   Due to the pre-installed SSM Agent on Windows Server 2022 and properly attached IAM Role, AWS Systems Manager successfully registered both instances under **Managed nodes** with an **Online** status.
    *   *Real screenshots*:
        *   Managed nodes showing Online: ![Fleet Manager Managed Nodes](/images/worklog/week-12/5_fleet_manager_managed_nodes.png)

*   **Scan and Install OS Updates via Patch Manager**:
    *   Used **Patch Manager** to trigger a **Scan and install** operation immediately to evaluate missing patches and install OS security baseline updates.
    *   Selected the `Do not reboot my instances` option to prevent service interruptions and selected both Windows nodes manually.
    *   The patching execution completed successfully.
    *   *Real screenshots*:
        *   Patch now configuration: ![Patch Now Config](/images/worklog/week-12/6_patch_now_config.png)
        *   Patching baseline Succeeded: ![Patching Succeeded](/images/worklog/week-12/7_patch_manager_succeeded.png)

*   **Execute Remote PowerShell Script via Run Command**:
    *   Used **Run Command** with the `AWS-RunPowerShellScript` document to execute `net user` on both Windows instances without requiring an RDP session.
    *   Configured S3 log storage to write standard execution outputs to `ssm-runcommand-logs-trantrung04` S3 bucket.
    *   The command ran successfully, returning active local user accounts: `Administrator`, `Guest`, `DefaultAccount`, `WDAGUtilityAccount`.
    *   *Real screenshots*:
        *   Run Command configuration: ![Run Command Config](/images/worklog/week-12/8_run_command_config.png)
        *   Run Command execution success: ![Run Command Success](/images/worklog/week-12/9_run_command_success_status.png)
        *   Execution output logs: ![Run Command Output](/images/worklog/week-12/10_run_command_output_logs.png)

#### 2. Resource Cleanup

To prevent any unexpected charges and ensure cost optimization under the AWS Free Tier, all resources were completely cleaned up immediately after testing:
*   **Terminated** both EC2 instances: `Windows-Lab-SSM-1` and `Windows-Lab-SSM-2`.
*   Deleted key pairs: `Windows-Lab-SSM-1-Key` and `Windows-Lab-SSM-2-Key` (including local `.pem` files).
*   Emptied and deleted the `ssm-runcommand-logs-trantrung04` S3 bucket.
*   Removed IAM Role associations, detached SSM policy, and deleted IAM Role `Windows-Lab-SSM-Role` and Instance Profile.
*   Deleted public subnets, Internet Gateway, and VPC `windows-lab-ssm`.


