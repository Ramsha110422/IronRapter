 # Creating Windows & Linux ECS Instances and Making Image

## Overview
This lab teaches how to create and manage Windows and Linux Elastic Cloud Servers (ECS) in Huawei Cloud, configure networking, and create custom system disk images. You'll also learn how to share these images across different projects using Huawei Cloud's Image Management Service.

## What is Lab Desktop?
Lab Desktop is a virtual desktop inside Huawei Cloud that lets you access cloud services via a browser. Use Google Chrome within this environment and log in to the Huawei Cloud Console using the IAM credentials provided at the top of the lab manual.

## Part 1: Creating Windows and Linux ECS Instances
### 1.1 What is a VPC?
A Virtual Private Cloud (VPC) is an isolated network in the cloud where you can launch ECS instances. It provides secure communication and control over networking components like subnets and gateways.

## Steps to Create a VPC
Go to AP-Singapore region.

Open Service List > Virtual Private Cloud > Create VPC.

Fill in:

VPC Name: system-assigned (e.g., vpc-Sandbox-abc123)

IPv4 CIDR Block: 192.168.0.0/16

Subnet Name: system-assigned

Subnet CIDR: 192.168.0.0/24

## Steps to Create Windows ECS
Go to Compute > Elastic Cloud Server, click Buy ECS.

Use settings:

Billing Mode: Pay-per-use

Architecture: x86

Flavor: s6.large.2 (2 vCPUs, 4GB RAM)

Image: Windows Server 2012 R2 64-bit

Disk: 40 GB High I/O

VPC: Select the one you created

ECS Name: ecs-windows

Password: Huawei@1234

Click Submit.

Wait until status is Running.

Steps to Create Linux ECS
Repeat the steps above with changes:

Image: CentOS 7.6 64-bit

ECS Name: ecs-linux

Assign an EIP (Elastic IP)

1.2 Logging In to ECS
For Windows ECS:
Use Remote Login > Log In

Click Send Ctrl+Alt+Del

Use Input Commands to paste password (Huawei@1234)

Press Enter

For Linux ECS:
Use Remote Login (VNC)

Username: root

Password: Huawei@123

### 1.3 Modify ECS Specifications
Stop ECS (Force Stop if needed)

Click More > Modify Specifications

Change RAM to 8GB

Click Submit and wait for Resized

Start ECS again

## Part 2: Create Windows System Disk Image
### 2.1 Configure Windows ECS
Enable DHCP:

Go to Network settings → Enable "Obtain IP address automatically"

Enable Remote Desktop:

Right-click This PC > Properties > Remote Settings

Allow RDP in Firewall

Ensure Cloudbase-Init is installed

### 2.2 Create System Disk Image
Go to Image Management Service > Create Image

Source: ecs-windows

Name: image-windows2012

Submit and wait for Status: Normal

### Part 3: Create Linux System Disk Image
## 3.1 Configure Linux ECS
Enable DHCP:

bash
Copy
Edit
vi /etc/sysconfig/network-scripts/ifcfg-eth0
Add:

ini
Copy
Edit
PERSISTENT_DHCLIENT="y"
Verify CloudResetPwdAgent:

bash
Copy
Edit
ls -lh /Cloud*
Verify cloud-init:

bash
Copy
Edit
rpm -qa | grep cloud-init
If missing:

bash
Copy
Edit
yum install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
yum install -y cloud-init
Clean UDEV rules (optional):

bash
Copy
Edit
ls -l /etc/udev/rules.d
