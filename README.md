# CloudFormation
AWS EC2 Deployment Using CloudFormation and CLI
AWS EC2 Deployment Using CloudFormation and CLI

Project Level: Beginner → Intermediate
Skills Demonstrated: AWS CLI, CloudFormation, EC2, Security Groups, SSH access, Infrastructure as Code (IaC), resource lifecycle management

Project Overview

This project demonstrates deploying an EC2 instance entirely using AWS CLI and CloudFormation, including:

Installing AWS CLI silently on Windows

Creating and deploying a CloudFormation stack

Configuring Security Groups for SSH access

Connecting to EC2 via SSH

Cleaning up resources to avoid charges

Step 1: Install AWS CLI (Windows)

Install silently using Command Prompt:

msiexec.exe /i https://awscli.amazonaws.com/AWSCLIV2.msi /qn


Verify installation:

aws --version

Step 2: Configure AWS CLI
aws configure


Provide:

AWS Access Key ID

AWS Secret Access Key

Default region (e.g., us-east-1)

Output format (json)

Verify identity:

aws sts get-caller-identity

Step 3: Deploy CloudFormation Stack

Deploy EC2 via your YAML template (ec2.yaml):

aws cloudformation create-stack --stack-name EC2cloudFormation --template-body file://ec2.yaml
aws cloudformation describe-stacks --stack-name EC2cloudFormation

Step 4: Get EC2 Public IP
aws ec2 describe-instances --query "Reservations[*].Instances[*].{ID:InstanceId,IP:PublicIpAddress}" --output table


Example output:

----------------------------------------
|           DescribeInstances          |
+----------------------+---------------+
|          ID          |      IP       |
+----------------------+---------------+
|  i-04bf849806c3e7be8 |  44.223.5.78 |
+----------------------+---------------+

Step 5: SSH into EC2
ssh -i "C:\path\to\my-key-pair.pem" ec2-user@<PUBLIC_IP>


Example:

ssh -i "C:\Users\USER\Downloads\windows key.pem" ec2-user@44.223.5.78


⚠️ If you get a timeout, create a Security Group allowing SSH access.

Step 6: Create Security Group for SSH
aws ec2 create-security-group --group-name SSHAccess --description "Allow SSH from anywhere"


Note the GroupId returned, e.g., sg-0f602e2bc04f07ba7.

Allow inbound SSH:

aws ec2 authorize-security-group-ingress --group-id sg-0f602e2bc04f07ba7 --protocol tcp --port 22 --cidr 0.0.0.0/0


Attach Security Group to EC2 instance:

aws ec2 modify-instance-attribute --instance-id i-04bf849806c3e7be8 --groups sg-0f602e2bc04f07ba7


Retry SSH connection.

Step 7: Delete Resources

Delete EC2 stack:

aws cloudformation delete-stack --stack-name EC2cloudFormation
aws cloudformation wait stack-delete-complete --stack-name EC2cloudFormation


Delete Security Group:

aws ec2 delete-security-group --group-id sg-0f602e2bc04f07ba7

Outcome

Successfully deployed and accessed an EC2 instance using CloudFormation and AWS CLI

Practiced Infrastructure as Code (IaC)

Gained experience with security management, SSH access, and resource lifecycle management
