# Secure-Access-to-S3-from-an-EC2-Instance-Inside-a-Custom-VPC-Using-IAM-RoleProject Type: AWS Hands-on
#Tools Used: AWS Management Console, IAM, S3, EC2, VPC, PowerShell (Windows)
#Region Used: us-east-1
🧩 Objective

The goal of this project is to securely allow an EC2 instance inside a custom VPC to access an S3 bucket using an IAM Role instead of hard-coded credentials.

🧠 Concept Overview

S3 (Simple Storage Service): Used to store files (test.txt)

VPC (Virtual Private Cloud): Isolated network for EC2 instance

EC2 (Elastic Compute Cloud): Virtual machine that accesses S3

IAM Role: Provides permission for EC2 to access S3 securely

🏗️ Step-by-Step Implementation
Step 1 — Create S3 Bucket

Go to AWS Console → S3 → Create bucket

Bucket name: varsha-vpc-lab-bucket

Region: us-east-1

Keep Block all public access ON

Click Create bucket

Upload a sample file named test.txt

✅ Expected Result: test.txt visible in the S3 bucket.

Step 2 — Create VPC (VPC and more)

Go to VPC → Create VPC → VPC and more

Name: varsha-custom-vpc

IPv4 CIDR: 10.0.0.0/16

Public Subnet CIDR: 10.0.0.0/24

AZ: us-east-1a

NAT Gateways: None

VPC Endpoints: None

DNS Options: Enable both hostnames and resolution

Click Create VPC

✅ Expected Result: Custom VPC with public subnet and Internet Gateway created.

Step 3 — Create IAM Role for EC2

Go to IAM → Roles → Create role

Trusted entity: AWS Service → choose EC2

Attach policy: AmazonS3ReadOnlyAccess

Role name: EC2-S3-AccessRole

Click Create role

✅ Expected Result: Role appears under IAM Roles list.

Step 4 — Create Security Group

Go to EC2 → Security Groups → Create security group

Name: ec2-s3-sg3

VPC: varsha-custom-vpc

Inbound rule:

Type: SSH

Port: 22

Source: My IP

Click Create security group

✅ Expected Result: SG created with SSH allowed only from your IP.

Step 5 — Launch EC2 Instance

Go to EC2 → Launch Instances

Name: ec2-in-vpc-demo

AMI: Amazon Linux 2 (or Windows if required)

Instance type: t2.micro

VPC: varsha-custom-vpc

Subnet: varsha-custom-vpc-subnet-public1-us-east-1a

Auto-assign public IP: Enable

Security Group: ec2-s3-sg3

IAM Role: EC2-S3-AccessRole

Key Pair: Create or choose existing .pem key

Launch instance

✅ Expected Result: EC2 running and accessible via browser or PowerShell.

Step 6 — Connect to EC2

Option A: EC2 Instance Connect (Browser)

Go to EC2 → Connect → EC2 Instance Connect → Connect

Option B: PowerShell (Windows)

Open PowerShell

Navigate to key path:

cd C:\Users\Varsha\Downloads


Connect:

ssh -i "your-key.pem" ec2-user@<Public-IP>

Step 7 — Install AWS CLI (if not installed)

Download AWS CLI v2 for Windows
👉 https://aws.amazon.com/cli/

Install it, then restart PowerShell

Test:

aws --version

Step 8 — Verify S3 Access

Run the following in PowerShell or EC2 terminal:

aws s3 ls
aws s3 ls s3://varsha-vpc-lab-bucket
aws s3 cp s3://varsha-vpc-lab-bucket/test.txt C:\Users\Varsha\Desktop\test.txt
type C:\Users\Varsha\Desktop\test.txt


✅ Expected Result: The test.txt file downloads successfully.

Step 9 — Verify Role Identity

Run this command:

aws sts get-caller-identity


✅ Expected Result: Output shows IAM Role EC2-S3-AccessRole.

Step 10 — Cleanup

Delete all resources after testing:

Terminate EC2

Delete S3 Bucket and file

Delete IAM Role

Delete Security Group

Delete VPC

🧾 Result

Successfully accessed an S3 bucket from an EC2 instance inside a custom VPC securely using an IAM role (no access keys used).

💡 Key Learnings

IAM roles are safer than access keys.

EC2 can access S3 directly if permissions are attached.

A VPC isolates and secures your network.
