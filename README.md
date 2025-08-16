🚀 AWS EC2 Deployment Using CloudFormation and CLI

Project Level: Beginner → Intermediate

Skills Demonstrated: AWS CLI, CloudFormation, EC2, Security Groups, SSH access, Infrastructure as Code (IaC), Resource Lifecycle Management

📖 Project Overview

This project demonstrates deploying an EC2 instance entirely using AWS CLI and CloudFormation, including:

💻 Installing AWS CLI on Windows using CMD (you can also use powershell)

🏗️ Creating and deploying a CloudFormation stack

🔐 Configuring Security Groups for SSH access

🔑 Connecting to EC2 via SSH

🗑️ Cleaning up resources to avoid charges

🛠️ Deployment Steps

1️⃣ Install AWS CLI (Windows)
💻 Install using Command Prompt:

msiexec.exe /i https://awscli.amazonaws.com/AWSCLIV2.msi

Verify installation:

aws --version

2️⃣ Configure AWS CLI
⚙️ Run aws configure and provide:

AWS Access Key ID

AWS Secret Access Key

Default region (e.g., us-east-1)

Output format (json)

Verify identity:

aws sts get-caller-identity

3️⃣ Deploy CloudFormation Stack
🏗️ Deploy EC2 via your YAML template (ec2.yaml):

aws cloudformation create-stack --stack-name EC2cloudFormation --template-body file://ec2.yaml

aws cloudformation describe-stacks --stack-name EC2cloudFormation

4️⃣ Get EC2 Public IP
🌐 Retrieve your instance's public IP:

aws ec2 describe-instances --query "Reservations[*].Instances[*].{ID:InstanceId,IP:PublicIpAddress}" --output table

Example output:

| DescribeInstances |
+----------------------+---------------+
| ID | IP |
+----------------------+---------------+
| i-04bf849806c3e7be8 | 44.223.5.78 |
+----------------------+---------------+

5️⃣ SSH into EC2
🔑 Connect using your key pair:

ssh -i "C:\path\to\my-key-pair.pem" ec2-user@<PUBLIC_IP>

Example:

ssh -i "C:\Users\USER\Downloads\windows key.pem" ec2-user@44.223.5.78

⚠️ If you get a timeout, create a Security Group allowing SSH access.

6️⃣ Create Security Group for SSH
🔐 Create a security group:

aws ec2 create-security-group --group-name SSHAccess --description "Allow SSH from anywhere"

Note the GroupId returned, e.g., sg-0f602e2bc04f07ba7.

Allow inbound SSH:

aws ec2 authorize-security-group-ingress --group-id sg-0f602e2bc04f07ba7 --protocol tcp --port 22 --cidr 0.0.0.0/0

Attach Security Group to EC2 instance:

aws ec2 modify-instance-attribute --instance-id i-04bf849806c3e7be8 --groups sg-0f602e2bc04f07ba7

Retry SSH connection.

7️⃣ Delete Resources
🗑️ Clean up to avoid charges:

Delete EC2 stack:

aws cloudformation delete-stack --stack-name EC2cloudFormation
aws cloudformation wait stack-delete-complete --stack-name EC2cloudFormation

Delete Security Group:

aws ec2 delete-security-group --group-id sg-0f602e2bc04f07ba7

✅ Outcome

🚀 Successfully deployed and accessed an EC2 instance using CloudFormation and AWS CLI

🛠️ Practiced Infrastructure as Code (IaC)

🔐 Gained experience with security management, SSH access, and resource lifecycle management
