---
title: "Week 3 Worklog"
date: 2026-05-04
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Week 3 Objectives:

* Learn about Amazon EC2 services on AWS.
* Practice initializing, configuring, and managing EC2 Instances.
* Deploy Node.js applications on Linux and Windows environments.

### Tasks to implement this week:

| Day    | Tasks                                                                                                                                                                                                                     | Start Date | Completion Date | Reference Sources                            |
| ------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2      | - Overview of Amazon EC2 <br> - Learn about Instance Types, AMI, Key Pair, and Snapshot <br> - Understand the mechanism of EC2 Instances                                                                            | 05/04/2026   | 05/04/2026      | [Amazon EC2 Basics](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html) <br> [FCJ Lab](https://000004.awsstudygroup.com/) |
| 3      | - Prepare EC2 environment <br>&emsp; + Create VPC for Linux <br>&emsp; + Create VPC for Windows <br>&emsp; + Create Security Group for Linux and Windows Instances                                                                  | 05/05/2026   | 05/05/2026      | [EC2 Security Groups](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-security-groups.html) <br> [FCJ Lab](https://000004.awsstudygroup.com/) |
| 4      | - Initialize Windows EC2 Instance <br>&emsp; + Create Windows Instance <br>&emsp; + Connect via Remote Desktop to Windows Instance                                                                                                 | 05/06/2026   | 05/06/2026      | [EC2 Windows Guide](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/concepts.html) <br> [FCJ Lab](https://000004.awsstudygroup.com/) |
| 5      | - Initialize Linux EC2 Instance <br>&emsp; + Create Linux Instance <br>&emsp; + Connect via SSH to Linux Instance <br> - Learn about changing EC2 configuration and managing EBS Snapshot                                                 | 05/07/2026   | 05/07/2026      | [EC2 Linux Guide](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html) <br> [FCJ Lab](https://000004.awsstudygroup.com/) |
| 6      | - Advanced EC2 Practice <br>&emsp; + Create Custom AMI <br>&emsp; + Create Instance from Custom AMI <br>&emsp; + Practice EC2 access when losing Key Pair                                                                        | 05/08/2026   | 05/08/2026      | [Amazon Machine Images (AMI)](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html) <br> [FCJ Lab](https://000004.awsstudygroup.com/) |
| 7 - Sun | - Deploy Node.js application on EC2 <br>&emsp; + Install LAMP/XAMPP Server <br>&emsp; + Install Node.js on Linux and Windows <br>&emsp; + Deploy Node.js application <br> - Learn about resource limits using IAM | 05/09/2026   | 05/10/2026      | [IAM for Amazon EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-policies-for-amazon-ec2.html) <br> [FCJ Lab](https://000004.awsstudygroup.com/) |

### Week 3 Achievements:

#### Theoretical:

* Understand the concepts and mechanism of Amazon EC2 in AWS.
* Master key EC2 components:
  * Instance Types
  * AMI
  * EBS Volume
  * Snapshot
  * Key Pair
  * Security Group
* Understand how to manage and initialize EC2 Instances in Linux and Windows environments.
* Know how to create Custom AMI and initialize Instance from custom AMI.
* Understand the mechanism of authorization and resource limitation using IAM Policy.

#### Practical:

* EC2 Instance: Successfully created and configured Linux and Windows Instances on AWS.
* Remote Connection: Successfully performed SSH into Linux EC2 and Remote Desktop into Windows EC2.
* Storage Management: Created and managed EBS Snapshots, modified EC2 configurations, and attached EBS storage.
* Backup & AMI: Successfully created Custom AMI and deployed a new Instance from the created AMI.
* Access Recovery: Practiced handling Key Pair loss using SSM and User Data.
* Application Deployment: Successfully installed LAMP/XAMPP, Node.js and deployed Node.js application on EC2 Linux and Windows.
* IAM Policy: Practiced configuring Region limits, Instance Type limits, and AWS resource operation permissions.

---

### Week 3 Practice Details:

#### 1. Lab Overview
{{% notice info "Information" %}}
In this lab, we will deploy two Amazon EC2 instances running Microsoft Windows Server 2025 and Amazon Linux 2023. To prepare for the deployment, we need to set up a secure network environment through Amazon VPC and configure appropriate Security Groups for each instance type.
{{% /notice %}}

{{% notice warning "Security Note" %}}
Setting up separate VPCs and Security Groups for instances enhances security and network access control, while adhering to the principle of least privilege.
{{% /notice %}}

#### 2. Preparation Steps

##### 2.1 Create VPC for Linux Instance
{{% notice info "Information" %}}
Amazon Virtual Private Cloud (VPC) allows you to launch AWS resources into a defined virtual network. In this section, we will create a separate VPC for the Linux instance with public subnets so it can be accessed from the internet.
{{% /notice %}}

**Initialize VPC:**
- In the **Create VPC** interface, select **VPC and more** to create the VPC along with related resources.
- At **Name tag auto-generation**, enter **Linux**.
- Successful creation screen:

![Successful VPC creation screen](/images/worklog/week-3/1.png)

**Configure Subnets:**
- In the VPC interface, select **Subnets** from the left menu.
- In the Subnets configuration interface:
  - Select the **Public subnet**.
  - Select **Actions**.
  - Select **Edit subnet settings**.
- Enable the automatic assignment of public IP addresses:
  - Check **Enable auto-assign public IPv4 address**.
  - Click **Save**.
- Successful public subnet configuration screen:

![Successful public subnet configuration](/images/worklog/week-3/2.png)

##### 2.2 Create VPC for Windows Instance
{{% notice info "Information" %}}
We will create a separate VPC for the Windows instance similarly to how we did for the Linux instance.
{{% /notice %}}

**Initialize VPC:**
- Access the **AWS Management Console**, search for and select **VPC**.
- In the VPC interface, select **Your VPCs** -> **Create VPC**.
- Select **VPC and more**, in **Name tag auto-generation**, enter **Windows**.
- Successful creation screen:

![Successful Windows VPC creation screen](/images/worklog/week-3/3.png)

**Configure Subnets:**
- Similarly to section 2.1, enable the automatic assignment of public IP addresses for the Public subnets of the Windows VPC.
- Successful configuration screen:

![Successful public subnet configuration for Windows](/images/worklog/week-3/4.png)

##### 2.3 Create Security Group for Linux Instance
{{% notice info "Information" %}}
Security Groups act as virtual firewalls to control traffic. We will open necessary ports for Linux (SSH, HTTP, HTTPS, MySQL, Node.js).
{{% /notice %}}

**Steps:**
- Select **Security Groups** -> **Create security group**.
- **Name:** Linux-SG | **Description:** Security group for Linux instance | **VPC:** Linux-vpc.
- **Inbound rules:** Add rules for SSH (22), ICMP, HTTP (80), HTTPS (443), MySQL (3306), and Custom TCP (5000).

{{% notice tip "Pro Tip" %}}
Always assign Tags to your Security Group for easier management and categorization of resources.
{{% /notice %}}

![Successfully created Security Group for Linux instance](/images/worklog/week-3/6.png)

##### 2.4 Create Security Group for Windows Instance
{{% notice info "Information" %}}
For the Windows instance, we need to open the RDP port (3389) instead of SSH for remote control.
{{% /notice %}}

**Steps:**
- Similarly to section 2.3, but select the Windows VPC.
- **Inbound rules:** Ensure the **RDP (3389)**, HTTP, HTTPS, and ICMP ports are open.

![Successfully created Security Group for Windows instance](/images/worklog/week-3/5.png)

{{% notice warning "Warning" %}}
Only open the ports truly necessary to minimize the attack surface of your system.
{{% /notice %}}

#### 3. Launch Windows Instance

##### 3.1 Create Windows Instance
{{% notice info "Information" %}}
Amazon EC2 provides Microsoft Windows Server 2025 as an operating system option for enterprise workloads on the AWS Cloud. EC2 currently supports Windows Server 2025 AMIs with License Included.
{{% /notice %}}

{{% notice warning "Warning" %}}
Launching Windows instances may incur costs. Be sure to terminate instances when no longer in use to avoid unnecessary charges.
{{% /notice %}}

**Steps:**
- Access the EC2 Console, select **Launch instances**.
- **Name:** Windows-instance.
- **AMI:** Select **Windows** -> **Microsoft Windows Server 2025 Base**.
- **Key pair:** Select **Create new key pair**.
  - **Key pair name:** kp-windows.
  - **Private key file format:** .pem.
  - Download and store the `.pem` file safely.
- **Network settings:** Select **Edit**.
  - **VPC:** Windows-vpc.
  - **Subnet:** Public subnet.
  - **Auto-assign public IP:** Enable.
  - **Security groups:** Select existing security group -> **Windows-SG**.
- Review and select **Launch instance**.

![Instance launched successfully](/images/worklog/week-3/7.png)

##### 3.2 Connect to Windows Instance
{{% notice info "Information" %}}
Connecting to a Windows instance on AWS EC2 is done via Remote Desktop Protocol (RDP) over port 3389. AWS provides a security mechanism to retrieve the administrator password using the previously created key pair.
{{% /notice %}}

**Steps:**
- In the EC2 interface, select **Windows-instance** -> **Connect**.
- Select the **RDP Client** tab -> **Get password**.
- Select **Browse** and upload the `kp-windows.pem` file, then select **Decrypt password**.
- Copy the displayed password.
- Download the **Remote desktop file** to your computer and open it.
- Enter the decrypted password to connect.
- Select **Yes** when asked about the security certificate.

![RDP connection screen](/images/worklog/week-3/8.png)

![Successfully connected to Microsoft Windows Server 2025 instance](/images/worklog/week-3/9.png)

##### 3.3 Prepare Sysprep for Custom AMI
{{% notice info "Information" %}}
Sysprep (System Preparation) is a Microsoft tool that helps prepare a Windows system for imaging. It removes SIDs and specific info so instances can be cloned without conflicts.
{{% /notice %}}

**Steps:**
- In the Windows search box, type and select **EC2LaunchSettings**.
- Under **Administrator Password setting**, select **Random**.
- At the bottom, select **Shutdown with Sysprep**.
- Confirm **Yes** for Windows to execute Sysprep and automatically shut down.

![Configure EC2Launch Settings](/images/worklog/week-3/10.png)

{{% notice warning "Warning" %}}
If Sysprep is not performed before creating an AMI, new instances launched from that AMI may encounter a "Password Not Available" error.
{{% /notice %}}

#### 4. Launch Linux Instance

##### 4.1 Create Linux Instance
{{% notice info "Information" %}}
Amazon Linux 2023 is a Linux operating system developed by AWS, specially optimized for cloud computing environments, providing high performance, security, and stability.
{{% /notice %}}

{{% notice warning "Warning" %}}
Launching EC2 instances may incur costs on your AWS account. Be sure to terminate instances when no longer in use.
{{% /notice %}}

**Steps:**
- Access the EC2 Console, select **Launch instances**.
- **Name:** Linux-instance.
- **AMI:** Select **Amazon Linux** -> **Amazon Linux 2023 AMI**.
- **Key pair:** Select **Create new key pair**.
  - **Key pair name:** kp-linux.
  - **Key pair type:** RSA.
  - **Private key file format:** .pem.
- **Network settings:** Select **Edit**.
  - **VPC:** Linux-vpc.
  - **Subnet:** Public subnet.
  - **Auto-assign public IP:** Enable.
  - **Security groups:** Select **Linux-SG**.
- Select **Launch instance**.

![Linux instance launch completed](/images/worklog/week-3/11.png)

{{% notice tip "Pro Tip" %}}
**"2/2 checks passed"** status check confirms that both system and instance checks are successful.
{{% /notice %}}

![Instance Running state](/images/worklog/week-3/12.png)

##### 4.2 Connect to Linux Instance
{{% notice info "Information" %}}
There are multiple methods to connect to an EC2 Linux instance. In this lab, we will learn two common methods: using **MobaXterm** and **PuTTY**.
{{% /notice %}}

**Method 1: Using MobaXterm**
- Open MobaXterm, select **Session** -> **SSH**.
- **Remote host:** Enter the Public IPv4 address of the instance.
- **Specify username:** ec2-user.
- **Advanced SSH settings:** Check **Use private key** and point to the `kp-linux.pem` file path.
- Click **OK** and select **Accept** for the first-time connection.

![MobaXterm interface](/images/worklog/week-3/13.png)

Successful connection

![Successful connection via MobaXterm](/images/worklog/week-3/14.png)

**Method 2: Using PuTTY**
{{% notice warning "Security Note" %}}
PuTTY does not directly support the `.pem` key format from AWS. You need to use **PuTTYgen** to convert it to `.ppk` format.
{{% /notice %}}

- **Key Conversion:**
  - Open PuTTYgen, click **Load** -> select the `kp-linux.pem` file.
  - Click **Save private key** and save it as `kp-linux.ppk`.
- **PuTTY Configuration:**
  - **Host Name:** Enter `ec2-user@<Public-IP>`.
  - **Connection** -> **SSH** -> **Auth** -> **Credentials**: Select the created `.ppk` file.
  - Go back to **Session**, name it, **Save**, and then click **Open**.

Successful connection

![Successful connection via PuTTY](/images/worklog/week-3/15.png)

**Check Internet Connectivity:**
- Use the `ping 8.8.8.8` command to verify.

![Ping test successful](/images/worklog/week-3/16.png)

#### 5. Amazon EC2 Basics
{{% notice info "Information" %}}
This practice provides an overview of working with Amazon EC2 objects and related components. We will focus on basic tasks such as changing configurations, creating snapshots, building custom AMIs, and accessing when losing key pairs.
{{% /notice %}}

{{% notice warning "Security Note" %}}
Throughout these practices, you will learn standard security practices for managing EC2 Instances, including secure key pair management and alternative access methods in case of lost credentials.
{{% /notice %}}

##### 5.1 Change EC2 Configuration
{{% notice info "Information" %}}
Selecting the right Amazon EC2 Instance Type is a crucial decision that directly affects the compute power of the virtual server. The Instance Type you choose will impact several key performance factors such as CPU, RAM, Network, and Storage.
{{% /notice %}}

**Steps:**
- In the EC2 interface, select **Windows-instance**.
- Ensure the instance is in the **Stopped** state. If not, select **Instance state** -> **Stop instance** and wait for it to stop completely.
- Select **Actions** -> **Instance settings** -> **Change instance type**.
- In the **New instance type** field, search for and select **t3.medium**.
- Select **Apply** (or Change).
- After successfully changing the instance type, select **Instance state** -> **Start instance**.

![Successfully changed instance type](/images/worklog/week-3/17.png)

{{% notice warning "Warning" %}}
To change the instance type, you must stop the instance first. This will interrupt the services and any applications running on the instance. Make sure you have planned for this downtime.
{{% /notice %}}

##### 5.2 Create and Manage EBS Snapshots
{{% notice info "Information" %}}
Amazon EBS Snapshots are point-in-time copies of your volumes, stored incrementally on Amazon S3. Snapshots are a critical component of backup and disaster recovery strategies.
{{% /notice %}}

**Steps:**
- In the EC2 interface, select **Snapshots** -> **Create snapshot**.
- In the **Create snapshot** interface:
  - **Resource type:** Select **Instance**.
  - **Instance:** Select **Windows-instance**.
- In the **Volumes** section, you can select **Copy tags from source volume**.
- Select **Create snapshot**.
- Wait a few minutes for the Snapshot status to change to **Completed**.

![Successfully created Snapshot](/images/worklog/week-3/18.png)

{{% notice info "Note" %}}
When selecting **Instance** as the Resource type, AWS will snapshot all EBS volumes attached to that EC2 instance. Selecting **Volume** targets only the specific disk you selected.
{{% /notice %}}

{{% notice tip "Pro Tip" %}}
Use snapshots to create periodic backups for important data. You can automate this process using **AWS Backup** or **Amazon Data Lifecycle Manager (DLM)**.
{{% /notice %}}

{{% notice warning "Security Note" %}}
Snapshots contain all data from the volume, so ensure appropriate access management to protect sensitive information.
{{% /notice %}}

##### 5.3 Create Custom AMI
{{% notice info "Information" %}}
An Amazon Machine Image (AMI) is a template that contains the software configuration (operating system, application, and settings) required to launch an EC2 instance. Creating a Custom AMI allows you to save the current state of an instance for reuse or to deploy multiple identical instances.
{{% /notice %}}

**Steps:**
- In the EC2 interface, select **Windows-instance**.
- Ensure the instance is in the **Stopped** state (to ensure Sysprep is fully processed).
- Select **Actions** -> **Image and templates** -> **Create image**.
- In the **Create image** interface:
  - **Image name:** `Custom Windows AMI`.
  - **Image description:** `Custom Windows AMI`.
  - **No reboot:** Uncheck (or select based on your downtime requirements).
- Select **Create image**.
- Access the **AMIs** section in the left menu to monitor the creation process.

![AMI creation completed](/images/worklog/week-3/19.png)

{{% notice warning "Warning" %}}
In this lab, the EC2 instance must be completely in the **Stopped** state to ensure the Sysprep function configured earlier is fully processed before creating a new AMI.
{{% /notice %}}

{{% notice tip "Pro Tip" %}}
Unchecking the **Reboot instance** option prevents the automatic reboot of the EC2 instance during AMI creation, which helps avoid Public IP changes (if not using an Elastic IP).
{{% /notice %}}

{{% notice warning "Security Note" %}}
Custom AMIs contain all data and configurations. Ensure you remove any sensitive information (credentials, private keys) before creating an AMI if you intend to share it.
{{% /notice %}}

##### 5.4 Launch Instance from Custom AMI
{{% notice info "Information" %}}
Amazon Machine Images (AMIs) allow you to create new EC2 instances with pre-configured settings and software. In this lab, we will use the previously created custom AMI to launch a new EC2 instance.
{{% /notice %}}

**Steps:**
- In the EC2 interface, select **AMIs**.
- Select the **Custom Windows AMI** created earlier and click **Launch instance from AMI**.
- **Name:** `Windows Server AMI`.
- **Key pair:** Select **Create new key pair**.
  - **Key pair name:** `kp-windows2`.
  - **Private key file format:** `.pem`.
- **Network settings:** Select **Edit**.
  - **VPC:** Select `Windows-vpc`.
  - **Subnet:** Select `public subnet`.
  - **Auto-assign public IP:** `Enable`.
  - **Security groups:** Select existing security group -> `Windows-SG`.
- Review the configuration and select **Launch instance**.

![New instance launched from AMI successfully](/images/worklog/week-3/20.png)

{{% notice tip "Pro Tip" %}}
**"3/3 checks passed"** status check confirms that system, instance, and reachability checks are all successful. The instance is ready to connect.
{{% /notice %}}

{{% notice warning "Security Note" %}}
Using a new key pair (`kp-windows2`) enhances security for the new instance launched from the AMI.
{{% /notice %}}

#### 6. Deploy Node.js Application on Amazon Linux
{{% notice info "Information" %}}
In this lab, we will deploy the AWS User Management application - a web application built using Node.js, Express, Express-Handlebars, and MySQL. This application allows for basic CRUD (Create, Read, Update, Delete) operations and searching.
{{% /notice %}}

{{% notice tip "Pro Tip" %}}
Amazon Linux 2023 is a Linux distribution specially optimized by AWS for cloud computing environments, providing high performance and deep integration with other AWS services.
{{% /notice %}}

{{% notice warning "Security Note" %}}
In a real production environment, you should consider separating the database by using **Amazon RDS** instead of installing MySQL directly on the EC2 instance to ensure high availability and better security.
{{% /notice %}}

##### 6.1 Install LAMP Web Server

###### 6.1.1 Prepare LAMP Server
After connecting to the Amazon Linux 2023 instance, we proceed to install the LAMP Stack.

**Steps:**
- Update the system:
  ```bash
  sudo dnf update -y
  ```
  ![dnf update](/images/worklog/week-3/21.png)

- Install Apache web server and PHP:
  ```bash
  sudo dnf install -y httpd wget php-fpm php-mysqli php-json php php-devel
  ```
  ![dnf install httpd php](/images/worklog/week-3/22.png)

- Install MariaDB server:
  ```bash
  sudo dnf install mariadb105-server
  ```
  ![dnf install mariadb](/images/worklog/week-3/23.png)

- Start and enable Apache to run at system boot:
  ```bash
  sudo systemctl start httpd
  sudo systemctl enable httpd
  sudo systemctl is-enabled httpd
  ```
  ![systemctl enable httpd](/images/worklog/week-3/24.png)

- Verify Apache by accessing the Public IPv4 address in a browser:
  ![Check Apache via IP](/images/worklog/week-3/25.png)
  ![Check Apache via DNS](/images/worklog/week-3/26.png)

- Configure permissions for the `/var/www` directory:
  ```bash
  sudo usermod -a -G apache ec2-user
  sudo chown -R ec2-user:apache /var/www
  sudo chmod 2775 /var/www && find /var/www -type d -exec sudo chmod 2775 {} \;
  find /var/www -type f -exec sudo chmod 0664 {} \;
  ```

###### 6.1.2 Verify LAMP Server
We verify if PHP is working correctly by creating a `phpinfo.php` file.

**Steps:**
- Create a test PHP file:
  ```bash
  echo "<?php phpinfo(); ?>" > /var/www/html/phpinfo.php
  ```
- Copy the **Public IPv4 DNS** of the instance and paste it into a browser using the structure: `http://<public-dns>/phpinfo.php`
- The PHP info page confirms the LAMP server is functioning normally:
  ![Verify phpinfo](/images/worklog/week-3/27.png)

- Verify the installed packages using the command:
  ```bash
  sudo dnf list installed httpd mariadb-server php-mysqlnd
  ```

- Delete the `phpinfo.php` file for security reasons:
  ```bash
  rm /var/www/html/phpinfo.php
  ```

###### 6.1.3 Configure Database Server
We will secure the MariaDB server using the `mysql_secure_installation` command.

**Steps:**
- Start MariaDB:
  ```bash
  sudo systemctl start mariadb
  ```
- Run the security configuration utility:
  ```bash
  sudo mysql_secure_installation
  ```
- Apply the following settings:
  - **Enter current password for root:** Press **Enter** (default is empty).
  - **Set root password? [Y/n]:** Enter **Y** and set a password (e.g., `123Admin`).
  - **Remove anonymous users? [Y/n]:** Enter **Y**.
  - **Disallow root login remotely? [Y/n]:** Enter **Y**.
  - **Remove test database and access to it? [Y/n]:** Enter **Y**.
  - **Reload privilege tables now? [Y/n]:** Enter **Y**.

![MariaDB security configuration](/images/worklog/week-3/28.png)

- Enable MariaDB to start at system boot:
  ```bash
  sudo systemctl enable mariadb
  ```

{{% notice warning "Security Note" %}}
Be sure to record this root password for later use in configuring the Node.js application. In a production environment, you should create a dedicated user for the application instead of using the root account.
{{% /notice %}}

###### 6.1.4 Install phpMyAdmin
phpMyAdmin is a web-based database management tool. We will install it to manage MariaDB more easily.

**Steps:**
- Install required dependencies:
  ```bash
  sudo dnf install php-mbstring php-xml -y
  ```
- Restart Apache and PHP-FPM to apply the new configuration:
  ```bash
  sudo systemctl restart httpd
  sudo systemctl restart php-fpm
  ```
- Download and extract phpMyAdmin:
  ```bash
  cd /var/www/html
  wget https://www.phpmyadmin.net/downloads/phpMyAdmin-latest-all-languages.tar.gz
  mkdir phpMyAdmin && tar -xvzf phpMyAdmin-latest-all-languages.tar.gz -C phpMyAdmin --strip-components 1
  rm phpMyAdmin-latest-all-languages.tar.gz
  ```
- Access the following path: `http://<Public-IPv4-DNS>/phpMyAdmin`
- Log in with the `root` user and the password set in the previous step.
  ![phpMyAdmin login page](/images/worklog/week-3/30.png)

- **Configure Database for the application:**
  - Create a new database named `awsuser`.
  ![Create awsuser database](/images/worklog/week-3/31.png)
  - Select the `awsuser` database, go to the **SQL** tab, and run the following command to create the `user` table:
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
  ![User table creation completed](/images/worklog/week-3/32.png)

##### 6.2 Install Node.js on Linux
{{% notice info "Information" %}}
Node.js is a server-side JavaScript runtime environment that allows for building highly scalable web applications. We will use **nvm** (Node Version Manager) to manage and install Node.js versions.
{{% /notice %}}

{{% notice warning "Warning" %}}
Ensure you have configured the Security Group for the EC2 instance to allow connections on port **5000** (the default port for the application in this lab).
{{% /notice %}}

**Installation Steps:**
- Install Node Version Manager (nvm):
  ```bash
  curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.0/install.sh | bash
  ```
- Activate nvm:
  ```bash
  . ~/.nvm/nvm.sh
  ```
- Install the latest Node.js LTS version:
  ```bash
  nvm install --lts
  ```
- Verify that Node.js and npm have been successfully installed:
  ```bash
  node -v
  npm -v
  ```
  ![Node.js installation successful](/images/worklog/week-3/33.png)

##### 6.3 Deploy Application on Linux Instance
We will proceed to clone the source code from GitHub and deploy the application.

**Steps:**
- Install git:
  ```bash
  sudo dnf install git -y
  ```
- Clone the application repository:
  ```bash
  cd ~ec2-user
  git clone https://github.com/First-Cloud-Journey/000004-EC2.git
  cd 000004-EC2
  ```
- Initialize the project and configure `package.json`:
  ```bash
  npm init
  ```
  ![npm init configuration](/images/worklog/week-3/34.png)

- Install required dependencies:
  ```bash
  npm install express dotenv express-handlebars body-parser mysql
  ```

{{% notice tip "Fixing Security Vulnerabilities" %}}
If `npm audit` reports security vulnerabilities related to `nodemon`, try uninstalling and reinstalling the latest version:
```bash
npm uninstall --save-dev nodemon
npm install nodemon@latest --save-dev
```
{{% /notice %}}

- Configure the database in the `.env` file:
  ```bash
  touch .env
  vi .env
  ```
  File content for `.env`:
  ```env
  DB_HOST=localhost
  DB_NAME=awsuser
  DB_USER=root
  DB_PASS=123Admin
  ```
  ![Verify .env configuration](/images/worklog/week-3/35.png)

- Start the application:
  ```bash
  npm start
  ```

- Access the application via a browser on port **5000**: `http://<Public-IPv4-DNS>:5000`
  ![AWS FCJ Management UI via IP](/images/worklog/week-3/36.png)
  ![AWS FCJ Management UI via DNS](/images/worklog/week-3/37.png)

- **Insert SQL Dummy Data via phpMyAdmin:**
  Use the `INSERT INTO user ...` code to add sample data. Once complete, refresh the application page:
  ![Sample data displayed on the application](/images/worklog/week-3/38.png)

- **Verify application features:**
  - **View details:** ![View user](/images/worklog/week-3/39.png)
  - **Edit:** ![Edit user](/images/worklog/week-3/40.png)
  - **Add new:** ![Add user](/images/worklog/week-3/41.png)
  - **Search:** ![Search user](/images/worklog/week-3/42.png)
  - **Actual database:** ![Database after insert](/images/worklog/week-3/43.png)
  - **Console interface when running server:** ![Console interface](/images/worklog/week-3/44.png)
