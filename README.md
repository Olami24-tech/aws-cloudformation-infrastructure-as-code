# AWS CloudFormation Infrastructure as Code

## Overview

This project demonstrates Infrastructure as Code (IaC) using AWS CloudFormation to provision a multi-tier AWS environment.

The infrastructure is separated into multiple CloudFormation templates, allowing networking, compute, identity, database, scaling, and storage resources to be deployed and managed using the AWS CLI.

## Architecture

<img width="874" height="765" alt="AWS CloudFormation Architecture Diagram" src="https://github.com/user-attachments/assets/727ecc4f-7525-435a-b2ce-7fc500932272" />

The architecture uses multiple Availability Zones and separates resources across public, application, and data tiers.

## AWS Services & Components

| Service / Component | Purpose |
| --- | --- |
| AWS CloudFormation | Infrastructure provisioning |
| Amazon VPC | Network isolation |
| Public Subnets | Internet-facing resources |
| Private App Subnets | Application workloads |
| Private Data Subnets | Database resources |
| Internet Gateway | Public internet connectivity |
| Amazon EC2 | Bastion and application servers |
| EC2 Auto Scaling | Application scalability |
| AWS IAM | Roles and permissions |
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
```

## CloudFormation Templates

### `vpc.yaml`

Creates the networking layer, including the VPC, public and private subnets, routing, Internet Gateway, Bastion Host, and related security groups.

### `ec2.yaml`

Creates EC2 compute resources used by the application layer.

### `iam.yaml`

Defines IAM roles and permissions required by the infrastructure.

### `asg.yaml`

Creates the Auto Scaling configuration used to scale application instances.

### `rds.yaml`

Creates the Amazon RDS database resources for the data tier.

### `s3-bucket.yaml`

Creates Amazon S3 storage resources.

### `s3-static.yaml`

Configures Amazon S3 static website hosting.

## Deployment

The templates are deployed and managed using the AWS CLI.

### Validate a Template

Before deploying, validate the CloudFormation template:

```bash
aws cloudformation validate-template \
  --template-body file://templates/vpc.yaml
```

### Create a Stack

```bash
aws cloudformation create-stack \
  --stack-name my-vpc-stack \
  --template-body file://templates/vpc.yaml
```

### Deploy IAM Resources

For templates containing named IAM resources:

```bash
aws cloudformation create-stack \
  --stack-name my-iam-stack \
  --template-body file://templates/iam.yaml \
  --capabilities CAPABILITY_NAMED_IAM
```

### Check Stack Status

```bash
aws cloudformation describe-stacks \
  --stack-name my-vpc-stack
```

### Review Stack Events

CloudFormation stack events can be used to troubleshoot failed deployments:

```bash
aws cloudformation describe-stack-events \
  --stack-name my-vpc-stack
```

### Delete a Stack

```bash
aws cloudformation delete-stack \
  --stack-name my-vpc-stack
```

## Key Features

- Modular CloudFormation templates
- Multi-AZ networking
- Public and private subnet architecture
- Bastion Host access
- EC2 application tier
- Security-group-based access control
- EC2 Auto Scaling
- Amazon RDS database tier
- Amazon S3 static website hosting
- AWS CLI-based deployment

## Troubleshooting & Lessons Learned

During this project, I gained practical experience troubleshooting CloudFormation stack rollbacks, resource dependencies, security group rules, EC2 SSH connectivity, changing public IP addresses, and AWS networking.

### Bastion Host Connectivity

One issue involved troubleshooting SSH connectivity to the Bastion Host.

I verified the EC2 instance state, public IP address, key pair, and security group configuration. I discovered that my public IP address had changed while the Bastion Host security group was configured to allow SSH from a specific `/32` address.

After updating the SSH ingress rule with my current public IP address, connectivity was restored.

### CloudFormation Stack Rollbacks

I also used CloudFormation stack events to identify failed resources and troubleshoot template configuration errors.

This helped me understand how to trace deployment failures, correct resource configuration issues, and successfully redeploy the infrastructure.

## What I Learned

This project strengthened my understanding of:

- Infrastructure as Code
- AWS CloudFormation templates
- VPC and subnet design
- Multi-AZ architecture
- EC2 connectivity
- Bastion Host architecture
- Security Group relationships
- CloudFormation stack lifecycle management
- AWS CLI infrastructure management
- Troubleshooting failed infrastructure deployments

## Future Improvements

- Parameterise environment-specific values such as SSH source CIDRs
- Introduce nested CloudFormation stacks
- Add CloudWatch monitoring and alarms
- Add an Application Load Balancer
- Add automated CloudFormation validation
- Integrate CI/CD deployment
- Support multiple environments such as development, staging, and production

## Author

**Yusuf Olamilekan Oyedele**

Cloud Engineering | AWS | Infrastructure as Code
