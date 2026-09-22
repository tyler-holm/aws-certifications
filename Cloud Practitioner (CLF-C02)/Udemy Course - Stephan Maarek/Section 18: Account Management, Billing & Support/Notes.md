# Section 18: Account Management, Billing & Support

## Organizations

### Organizations Overview
- Global Service
- Allows to manage multiple AWS accounts
- Main account in the master account
- Other accounts are child accounts
- Cost Benefits:
    - Consolidated Billing across all accounts - single payment method
    - Pricing benefits from aggregated usage (volume discount for EC2, S3, etc.)
    - Pooling of reserved EC2 instances for optimal savings
- API is available to automate AWS account creation
- Restrict account privileges using Service Control Policies (SCP)
- Multi-Account Strategies
    - Alternative to creating separate VPCs for various groups.
    - Create accounts per department, per cost center, per dev/test/prod, based on regulatory restrictions (using SCP), for better resources isolation, to have separate per-account service limits, isolated account for logging
    - Can use tagging standards for billing purposing
    - Should enable cloudtrail on all accounts, send logs to central S3 account
    - Send CloudWatch Logs to central logging account

### Organizations Service Control Policies (SCP)
- Whitelist or blacklist IAM actions
- Applied at OU or account level
- Does not apply to the Master Account
- SCP is applied to all Users and Roles of the Account, including Root
- The SCP does not affect service-linked roles
    - Service-linked roles enable other AWS services to integrate with AWS Organization and can't be restricted by SCPs.
- Must have an explicit Allow (does not allow anything by default)
- Use cases:
    - Restrict access to certain services
    - Example: block production accounts from accessing EMR
    - Example: Enforce PCI compliance by explicitly disabling services

### Organizations Consolidated Billing
- When enabled provides you with:
    - Combined Usage - combine the usage across all AWS accounts in the AWS Organization to share the volume pricing, Reserved Instances and Savings Plans discounts
    - One bill - consolidated billing for all AWS Accounts in the AWS Organization
- The management account can turn off reserved instances discount sharing for any account in the AWS Organization, including itself

## AWS Control Tower
- Easy way to set up and govern a secure and compliant multi-account AWS environment based on best practices
- Benefits: 
    - Automate the set up of your environment in a few clicks
    - Automate ongoing policy management using guardrails
    - Detect policy violations and remediate them
    - Monitor compliance through an interactive dashboard
- AWS Control Tower runs on top of AWS Organizations:
    - automatically sets up AWS Organizations to organize accounts and implement SCPs

## AWS Resource Access Manager (RAM)
- Share AWS resources that you own with other AWS accounts
- Share with any account (in organization or not)
- Avoids resource duplication
- Supported resources include:
    - Aurora
    - VPC subnets
    - Transit Gateway
    - Route 53
    - EC2 Dedicated Hosts
    - License Manager configurations
    - and more

## AWS Service Catalog
- Problems solved by AWS service catalog:
    - Users that are new to AWS have too many options, and may create stacks that are not compliant / in line with the rest of the organization
    - Some users just want a quick self-service portal to launch a set of authorized products pre-defined by admins
- Includes:
    - Virtual machines
    - Databases
    - Storage options
    - etc

### How it works
- Admin creates a "Product"
    - Products are a CloudFormation template
- Multiple Products are grouped into a "portfolio"
- IAM Permissions define access to products within a portfolio
- Users can login and view a product list that contains all the products they're approve for
- Users can launch these products, and the resources automatically get provisioned by Cloudformation via the template

## AWS Pricing Models
- four pricing models
- Pay as you go:
    - pay for what you use, remain agile, responsive, meet scale demands
- Save when you reserve:
    - minimize risks, predictably manage budgets, comply with long-term requirements
    - Reservations are available for EC2 Reserved Instances, DynamoDB Reserved Capacity, ElastiCache Reserved Nodes, RDS Reserved Instances, Redshift Reserved Nodes
- Pay less by using more:
    - Volume based discounts 
- Pay less as AWS grows
    - As AWS grows they are able to save money and they pass this cost savings on to the users

### Free Services & Free Plan in AWS
- With a new AWS account, you get up to $200 in credits
- You choose between Free Plan or Paid Plan
- Free Plan expires in 6 months or when credits are consumed (no charges)
- Paid Plan charges you after you consume your credits
- Both have access to "Always Free Services"
- Always Free Service Examples:
    - Lambda - 1,000,000 request/month and 400,000 GB-seconds of compute per month for free
    - DynamoDB - 25 GB of storage and 200M requests per month

### Compute Pricing - EC2
- Only charged for what you use
- Number of instances
- Instance configuration:
    - Physical capacity
    - region
    - OS and software
    - instance type
    - instance size
- ELB running time and amount of data processed
- Detailed monitoring

- On-demand instances:
    - minimum of 60s
    - pay per second (linux/windows) or per hour (other)
- Reserved instances:
    - Up to 75% discount compared to On-Demand on hourly rate
    - 1 or 3 year commitment
    - All upfront, partial upfront, no upfront
- Spot instances:
    - up to 90% discount compared to On-Demand on hourly rate
    - Bid for unused capacity
    - risk of losing space if someone is willing to pay more
- Dedicated Host:
    - On-demand
    - reservation for 1 year or 3 year commitment
    - physical resources are not shared
- Savings plans:
    - alternative to save on sustained usage

### Computer Pricing - Lambda and ECS
- Lambda:
    - Pay per call
    - Pay per duration
- ECS:
    - EC2 Launch type model: No addition fees, you pay for AWS resources stored and created in you application
- Fargate:
    - Fargate Launch Type Model: Pay for vCPU and memory resources allocated to your applications in your containers

### Storage Pricing - S3
- Storage class: S3 standard, S3 infrequent access, S3 one-zone IA, S3 Intelligent Tiering, S3 Glacier, S3 Glacier Deep Archive
- Number and size of objects: price can be tiered (based on volume)
- Number and type of requests
- Data In is free, data out cost money
- S3 Transfer Acceleration
- Lifecycle transitions
- Similar service: EFS (pay per use, has infrequent access and lifecycle rules)

### Storage Pricing - EBS
- Volume type (based on performance)
- Storage volume in GB per month provisioned
- IOPS:
    - General purpose SSD: included
    - Provisioned IOPS SSD: not included, must be provisioned
    - Magnetic: Number of requests
- Snapshots:
    - Added data cost per GB per month
- Data transfer:
    - Outbound data transfer are tiered for volume discounts
    - Inbound is free

### Database Pricing - RDS
- Per hour billing
- Database characteristics
    - Engine
    - Size
    - Memory class
- Purchase type:
    - On-demand
    - Reserved instances (1 or 3 years) with optional up-front
- Backup Storage:
    - there is no additional charge for backup storage up to 100% of your total database storage for a region
- Additional Storage (per GB per month)
- Number of input and output requests per month
- Deployment type (storage and I/O are variable):
    - single AZ
    - multiple AZ
- Data transfer
    - outbound data transfers are tiered for volume discounts
    - inbound is free


### Content Delivery - CloudFront
- Pricing is different across different geographic regions
- Aggregated for each edge location, than applied to your bill
- Data transfer Out (volume discount)
- Number of HTTP/HTTPS requests

### Networking Costs in AWS per GB - Simplified
- traffic into an AZ is free
- traffic inside the private subnet of a single AZ is free
- traffic between AZs in a single region is $0.02 per GB if using a public IP
- traffic between AZs in a single region is $0.01 per GB if using a private IP
- Traffic between regions is $0.02 per GB (inter-region)