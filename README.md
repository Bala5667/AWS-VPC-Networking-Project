# AWS-VPC-Networking-Project

Key Objectives of the project:
•	Created a VPC with CIDR block 10.0.0.0/16.
•	Provisioned Subnets:
•	Public Subnet: 10.0.1.0/24 (For internet-facing resources).
•	Private Subnet: 10.0.2.0/24 (For internal workloads).
•	Configured an Internet Gateway (IGW) to allow public instances to access the internet.
•	Set up a NAT Gateway to enable outbound internet access for private instances while keeping them secure.

Created Route Tables:
•	Public Route Table → Routes traffic through IGW.
•	Private Route Table → Routes traffic through NAT Gateway.
•	Launched EC2 instances in both public and private subnets:
•	Public EC2 Instance: Assigned a public IP and configured for direct internet access.
•	Private EC2 Instance: No public IP, accesses the internet via NAT Gateway.

Configured Security Groups to allow appropriate inbound and outbound traffic.
Verified Network Connectivity:
•	Successfully SSH’d into the Public EC2 Instance.
•	Accessed the Private EC2 Instance via the Public EC2.
•	Tested outbound internet access from the Private EC2 via NAT Gateway


Steps to achive the objectives :

Step 1: Creating the VPC :

I navigated to AWS Management Console → VPC → Your VPCs.
Clicked Create VPC and provided the following details:
VPC Name: My-VPC
IPv4 CIDR Block: 10.0.0.0/16 (to get 65,536 IPs).
Clicked Create VPC.

Step 2: Creating and Attaching an Internet Gateway
Went to VPC → Internet Gateways.
Clicked Create Internet Gateway, named it My-IGW, and created it.
Attached it to My-VPC by selecting it and clicking Attach to VPC.

Step 3: Creating Public and Private Subnets
Public Subnet:
Name: Public-Subnet
VPC: My-VPC
CIDR Block: 10.0.1.0/24
Availability Zone: us-east-1a
Private Subnet:
Name: Private-Subnet
VPC: My-VPC
CIDR Block: 10.0.2.0/24
Availability Zone: us-east-1a

Step 4: Creating and Configuring Route Tables

Public Route Table
Created a new Route Table named Public-RT and associated it with My-VPC.
Attached Public-Subnet to Public-RT.
Added a route to the internet:
Destination: 0.0.0.0/0
Target: My-IGW

Private Route Table
Created another Route Table named Private-RT and associated it with My-VPC.
Attached Private-Subnet to Private-RT.

Step 5: Creating Security Groups
Public Security Group (Public-SG)
Allowed:
SSH (22) from My IP
HTTP (80), HTTPS (443) from anywhere
RDP (3389) if needed for Windows

Private Security Group (Private-SG)
Allowed only traffic from Public-SG, including:
SSH (22) from Public-SG
Other required ports if needed

Step 6: Launching EC2 Instances

Public EC2
VPC: My-VPC
Subnet: Public-Subnet
Auto-assign Public IP: Enabled
Security Group: Public-SG
Key Pair: Used existing my-key.pem

Private EC2
VPC: My-VPC
Subnet: Private-Subnet
Auto-assign Public IP: Disabled
Security Group: Private-SG

Step 7: Creating and Configuring NAT Gateway
Created a NAT Gateway in Public-Subnet and attached a new Elastic IP.
Updated Private-RT to allow internet access via NAT Gateway:
Destination: 0.0.0.0/0
Target: My-NAT-Gateway

Step 8: Testing Internet Connectivity
Checking Public EC2 Connectivity
SSH into Public EC2
ssh -i "my-key.pem" ec2-user@<Public-EC2-Public-IP>
Verified internet access
Command :
curl ifconfig.me
ping -c 4 google.com

Step : Checking Private EC2 Connectivity:

Now, you can SSH into your Private EC2 instance from your Public EC2.
In your Public EC2 instance terminal, run:
ssh -i my-key.ppk ec2-user@<Private-EC2-Private-IP>

On Windows, transfer the my-key.pem file to the Public EC2 using WinSCP or PSCP.
Set Correct Permissions On the Public EC2, run:
chmod 400 my-key.pem

Connected to Private EC2 via Public EC2
Command :
ssh -i "my-key.pem" ec2-user@<Private-EC2-Private-IP>

Checked internet access via NAT Gateway :
curl ifconfig.me
ping -c 4 google.com
The output matched the NAT Gateway’s Elastic IP, confirming that the private instance accesses the internet via NAT.

Final Thoughts
✅ VPC with Public and Private Subnets successfully set up.
✅ Public EC2 connected to the internet directly.
✅ Private EC2 accessed the internet via NAT Gateway while remaining secure.
✅ Successfully tested connectivity from both instances.

This setup follows best practices for networking in AWS and provides a secure and scalable architecture. 🚀











