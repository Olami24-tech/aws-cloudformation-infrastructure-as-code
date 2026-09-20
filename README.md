# AWS CloudFormation Infrastructure as Code

## Overview

This project demonstrates Infrastructure as Code (IaC) using AWS CloudFormation to provision a multi-tier AWS environment.

The infrastructure is separated into multiple independent CloudFormation templates, allowing networking, compute, identity, database, scaling, and storage resources to be deployed and managed independently using the AWS CLI.

## Architecture

<img width="874" height="765" alt="Next js Architectural Diagram" src="https://github.com/user-attachments/assets/727ecc4f-7525-435a-b2ce-7fc500932272" />


## AWS Services Used

| Service | Purpose |
| --- | --- |
| AWS CloudFormation | Infrastructure provisioning |
| Amazon VPC | Network isolation |
| Public Subnets | Internet-facing resources |
| Private App Subnets | Application workloads |
| Private Data Subnets | Database resources |
| Internet Gateway | Public internet connectivity |
| Amazon EC2 | Bastion and application servers |
| Auto Scaling | Application scalability |
| IAM | Roles and permissions |
| Amazon RDS | Relational database |
| Amazon S3 | Storage and static website hosting |
| AWS CLI | Deployment and stack management |

## Project Structure

```text
aws-cloudformation-infrastructure-as-code/
├── templates/
│   ├── vpc.yaml
│   ├── ec2.yaml
│   ├── iam.yaml
│   ├── asg.yaml
│   ├── rds.yaml
│   ├── s3-bucket.yaml
│   └── s3-static.yaml
├── website/
│   └── index.html
├── diagrams/
├── screenshots/
├── README.md
└── .gitignore


## CloudFormation Templates

```
vpc.yaml

Creates the networking layer, including the VPC, public/private subnets, routing, Internet Gateway, Bastion Host and related security groups.

```
ec2.yaml

Creates EC2 compute resources used by the application layer.

```
iam.yaml

Defines IAM roles and permissions required by the infrastructure.

```
asg.yaml

Creates the Auto Scaling configuration used to scale application instances.

```
rds.yaml

Creates the Amazon RDS database resources for the data tier.

```
s3-bucket.yaml

Creates Amazon S3 storage resources.

```
s3-static.yaml

Configures S3 static website hosting.

## Deployment
The templates are deployed independently using the AWS CLI.
Before deploying, validate a template:
```
aws cloudformation validate-template \
  --template-body file://templates/vpc.yaml
Example deployment:
```
aws cloudformation create-stack \
  --stack-name my-vpc-stack \
  --template-body file://templates/vpc.yaml
For templates containing IAM resources:
```
aws cloudformation create-stack \
  --stack-name my-iam-stack \
  --template-body file://templates/iam.yaml \
  --capabilities CAPABILITY_NAMED_IAM
Check stack status:
```
aws cloudformation describe-stacks \
  --stack-name my-vpc-stack
Delete a stack:
```
aws cloudformation delete-stack \
  --stack-name my-vpc-stack

## Key Features
•	Modular CloudFormation templates 
•	Multi-AZ networking 
•	Public and private subnet architecture 
•	Bastion Host access 
•	EC2 application tier 
•	Security-group-based access control 
•	Auto Scaling 
•	Amazon RDS database tier 
•	S3 static website hosting 
•	AWS CLI based deployment 

## Troubleshooting & Lessons Learned
During this project I gained practical experience troubleshooting CloudFormation stack rollbacks, resource dependencies, security group rules, EC2 SSH access, changing public IP addresses, and AWS networking.
One example involved troubleshooting Bastion Host connectivity. I verified the EC2 instance state, public IP, key pair and security group configuration, then identified that my public IP address had changed. Updating the SSH ingress rule restored connectivity.
I also gained experience working with CloudFormation stack events to identify failed resources and correct template configuration errors.

## Future Improvements
•	Parameterise environment-specific values such as SSH source CIDRs 
•	Introduce nested stacks 
•	Add CloudWatch monitoring and alarms 
•	Add an Application Load Balancer 
•	Add automated CloudFormation validation 
•	Add CI/CD deployment 
•	Add additional environment support 


## Author
Yusuf Olamilekan Oyedele

