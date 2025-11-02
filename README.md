🚀 Secure Access to S3 from an EC2 Instance Inside a Custom VPC Using IAM Role

🎥 **Project Demo Video**  
[![Watch the video](

Project Type: AWS Hands-on
Tools Used: AWS Management Console, IAM, S3, EC2, VPC, PowerShell (Windows)
Region Used: us-east-1

🧩 Objective
The goal of this project is to securely allow an EC2 instance inside a custom VPC to access an S3 bucket using an IAM Role instead of hard-coded credentials.

🧠 Concept Overview
Component	Description
S3 (Simple Storage Service)	Used to store files (e.g., test.txt)
VPC (Virtual Private Cloud)	Provides an isolated network for EC2
EC2 (Elastic Compute Cloud)	Virtual machine that will access S3
IAM Role	Grants EC2 permission to access S3 securely

🏗️ Step-by-Step Implementation
Step 1 — 
	Create S3 Bucket
	Go to AWS Console → S3 → Create bucket
	Bucket name: varsha-vpc-lab-bucket
	Region: us-east-1
	Keep “Block all public access” enabled
	Click Create bucket
	Upload a sample file — test.txt

✅ Expected Result: test.txt is visible in the S3 bucket.

Step 2 — 
	Create VPC
	Go to VPC → Create VPC → VPC and more
	Name: varsha-custom-vpc
	IPv4 CIDR: 10.0.0.0/16
	Public Subnet CIDR: 10.0.0.0/24
	Availability Zone: us-east-1a
	NAT Gateways: None
	VPC Endpoints: None
	Enable DNS hostnames and DNS resolution
	Click Create VPC

✅ Expected Result: A custom VPC with one public subnet and Internet Gateway is created.

Step 3 — 
	Create IAM Role for EC2
	Go to IAM → Roles → Create role
	Trusted entity: AWS Service → Select EC2
	Attach Policy: AmazonS3ReadOnlyAccess
	Role name: EC2-S3-AccessRole
	Click Create role

✅ Expected Result: Role appears under IAM Roles.

Step 4 — 
	Create Security Group
	Go to EC2 → Security Groups → Create security group
	Name: ec2-s3-sg3
	VPC: varsha-custom-vpc
	Add Inbound Rule:
	Type: SSH
	Port: 22
	Source: My IP
	Click Create security group

✅ Expected Result: Security Group allows only SSH from your IP.

Step 5 — 
	Launch EC2 Instance
	Go to EC2 → Launch Instances
	Name: ec2-in-vpc-demo
	AMI: Amazon Linux 2 (or Windows if needed)
	Instance Type: t2.micro
	VPC: varsha-custom-vpc
	Subnet: varsha-custom-vpc-subnet-public1-us-east-1a
	Auto-assign Public IP: Enable
	Security Group: ec2-s3-sg3
	IAM Role: EC2-S3-AccessRole
	Key Pair: Choose or create a .pem key
	Click Launch Instance

✅ Expected Result: EC2 instance is running and reachable.

Step 6 — 
	Connect to EC2
	Option A — Using EC2 Instance Connect (Browser)
	Go to EC2 → Connect → EC2 Instance Connect → Connect
	
	
	Option B — Using PowerShell (Windows)
	cd C:\Users\Varsha\Downloads
	ssh -i "your-key.pem" ec2-user@<Public-IP>

Step 7 — 
	Install AWS CLI (if not installed)
	Download AWS CLI v2 for Windows:
	👉 https://aws.amazon.com/cli/
	Install and restart PowerShell
	Test installation:
	aws --version

Step 8 — Verify S3 Access
	Run these commands from EC2 or PowerShell:

	aws s3 ls
	aws s3 ls s3://varsha-vpc-lab-bucket
	aws s3 cp s3://varsha-vpc-lab-bucket/test.txt C:\Users\Varsha\Desktop\test.txt
	type C:\Users\Varsha\Desktop\test.txt

✅ Expected Result: The file test.txt downloads successfully.

	Step 9 — 
	Verify IAM Role Identity
	aws sts get-caller-identity

✅ Expected Result: The output shows IAM Role EC2-S3-AccessRole.

Step 10 — 
	Cleanup
	After testing, delete all created resources:
	Terminate EC2 instance
	Delete S3 bucket and file
	Delete IAM Role
	Delete Security Group
	Delete VPC

🧾 Final Result

✅ Successfully accessed an S3 bucket from an EC2 instance inside a custom VPC without using access keys, ensuring secure communication using IAM roles.
