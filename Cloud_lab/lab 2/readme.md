# Huawei Cloud Compute Service Lab: Creating, Managing, and Imaging ECS Instances
This lab exercise demonstrates how to utilize Huawei Cloud's Elastic Cloud Server (ECS) service to create and manage both Windows and Linux instances, configure networking, and create custom system disk images for reusability. It also guides you through sharing images across different projects, making it a complete end-to-end ECS experience for both beginners and intermediate users.

# What is Lab Desktop?
Lab Desktop is a virtualized desktop environment provided within the Huawei Cloud lab platform. It allows users to access cloud resources like ECS, VPC, and IAM accounts through a web browser interface. You can log into the Huawei Cloud Console via the Google Chrome browser inside Lab Desktop using the IAM User Login method and credentials provided at the top of the lab manual.

## 1. Creating Windows and Linux ECS Instances
### 1.1 What is a VPC?
A Virtual Private Cloud (VPC) is a secure, isolated network that you create in the cloud. It allows ECS instances to communicate securely while providing full control over IP addressing, subnets, routing, and security.

## Steps to Create a VPC:
Navigate to the AP-Singapore region in the Huawei Cloud Console.

Go to Service List > Virtual Private Cloud and click Create VPC.

### Configure:

VPC Name: Use system-assigned format (e.g., vpc-Sandbox-xyz123)

IPv4 CIDR Block: 192.168.0.0/16

Subnet Name: system-assigned format (e.g., subnet-Sandbox-xyz123)

Subnet CIDR Block: 192.168.0.0/24

## Steps to Create Windows ECS:
Go to Compute > Elastic Cloud Server, click Buy ECS.

### Set:

Billing Mode: Pay-per-use

Region: AP-Singapore

Architecture: x86

Flavor: s6.large.2 (2 vCPUs | 4GB)

Image: Windows Server 2012 R2 Standard 64-bit

System Disk: High I/O, 40 GB

Host Security: Enable (basic)

Network: Select your created VPC.

No EIP (Elastic IP) needed for Windows.

ECS Name: ecs-windows

Login Mode: Password (Huawei@1234)

Click Submit and wait for ECS status to become Running.

## Steps to Create Linux ECS:
Repeat the above steps with the following differences:

ECS Name: ecs-linux

Image: CentOS 7.6 64-bit

EIP: Auto assign

### 1.2 Logging In to ECS
Windows ECS:
Click Remote Login → Log In

Send Ctrl+Alt+Del to activate login screen

Paste password using Input Commands and press Enter

Upon successful login, Windows desktop appears

Linux ECS:
Use Remote Login via VNC

Login with:

Username: root

Password: Huawei@123

If welcome message appears, login is successful

### 1.3 Modifying ECS Specifications
You can modify ECS specs (like increasing memory):

Stop the ECS (use Force Stop if necessary)

Click More > Modify Specifications

Change memory from 4GB to 8GB

Submit and wait for status Resized

Start ECS again to apply changes

## 2. Creating a Windows System Disk Image
### 2.1 Configure Windows ECS:
Ensure DHCP is enabled:

Control Panel > Network & Sharing Center > Ethernet > TCP/IPv4 → Obtain IP/DNS Automatically

Enable Remote Connections:

Right-click This PC > Properties > Remote settings > Allow

Enable RDP in Firewall:

Control Panel > Firewall > Allow app → Allow RDP for public & private networks

Verify Cloudbase-Init is installed (default for public images)

### 2.2 Create a Private Image:
Go to Image Management Service > Create Image

Type: System disk image

Source: ecs-windows

Name: image-windows2012

Submit and wait (10–20 min) for status to be Normal

## 3. Creating a Linux System Disk Image
### 3.1 Configure Linux ECS:
Enable DHCP:

Edit /etc/sysconfig/network-scripts/ifcfg-eth0 and add:
PERSISTENT_DHCLIENT="y"

Verify CloudResetPwdAgent is installed:

bash
Copy
Edit
ls -lh /Cloud*
Verify Cloud-Init is installed:

bash
Copy
Edit
rpm -qa | grep cloud-init
If missing, install using:

bash
Copy
Edit
yum install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
yum install -y cloud-init
Clean network rule files:

bash
Copy
Edit
ls -l /etc/udev/rules.d
