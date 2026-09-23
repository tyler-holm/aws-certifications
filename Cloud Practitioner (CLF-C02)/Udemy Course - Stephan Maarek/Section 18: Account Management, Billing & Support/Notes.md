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

## Savings Plan Overview
- Commit a certain $ amount per hour for 1 or 3 years
- Easier way to setup long-term commitments on AWS
- EC2 Savings Plan
    - Up to 72% discount compared to On-Demand
    - Commit to usage of individual instance families in a region (C5 or M5)
    - Regardless of AZ, size (m5.xl to m5.4xl), OS, or tenancy
    - All upfront, partial upfront, no upfront
    - All upfront = bigger discount
- Compute Savings Plan
    - Up to 66% discount compared to On-Demand
    - Regardless of Family, Region, size, OS, tenancy, compute options
    - Compute Options: EC2, Fargate, Lambda
- Machine learning savings plan
    - Services like SageMaker
- Can be setup from the AWS Cost Explorer console
- AWS has a price estimate tool

## Compute Optimizer Overview
- Reduce costs and improve performance by recommending optimal AWS resources for your workloads
- Helps you choose optimal configurations and the right size for your workloads
- Users Machine Learning to analyze your resources' configurations and their utilization CloudWatch metrics
- Supported resources:
    - EC2 instances
    - EC2 Auto Scaling Groups
    - EBS volumes
    - Lambda functions
- Lower your costs by up to 25%
- Recommendations can be exported to S3

## Billing and Costing Tools
- Estimating costs in the cloud:
    - Pricing calculator
- Tracking costs in the cloud:
    - billing dashboard
    - cost allocation tags
    - cost and usage reports
    - cost explorer
- Monitoring against costs plans:
    - billing alarms
    - budgets

### Pricing Calculator
- https://calculator.aws/
- estimate the cost for your solution architecture

### AWS Billing Dashboard
- monthly cost summary
- forecasted month costs
- month to date costs

### Cost Allocation Tags
- used to track your AWS costs on a detailed level
- used for organizing resources
- AWS generated tags
    - start with "aws" prefix
    - automatically applied to created resources
- User defined tags
    - start with the "user" prefix
    - allow grouping by more abstract tags
        - application
        - owner
        - etc.
- Can be used to create resource groups
    - Create, maintain, and view a collection of resources that share common tags
    - manage common tags with the tag editor
- Can be applied to all resources created via CloudFormations template

### Cost and Usage Reports
- Dive deeper into your AWS costs and usage
- The AWS Cost and Usage Report contains the most comprehensive set of AWS cost and usage data available, including additional metadata about AWS services, pricing, and reservations.
- The AWS Cost and Usage Report lists AWS usage for each service category used by an account and its IAM users in hourly or daily line items, as well as any tags that you have activated for cost allocation purposes
- Can be integrated with Athena, Redshift, or QuickSight

### Cost Explorer
- Visualize, understand, and mange your AWS costs and usage over time
- Create custom reports that analyze cost and usage data
- Analyze your data at a high level: total costs and usage across all account
- explore at the monthly, hourly, or resource level
- Choose an optimal Savings Plan 
- EXAM NOTE: Forecast usage up to 12 months based on previous usage

### Billing Alarms in CloudWatch
- Billing data metric is stored in CloudWatch us-east-1
- Billing data is aggregated for global AWS costs
- Its for actual cost, not projected
- Intended as a simple alarm. (not as powerful as AWS budgets)

### AWS Budgets
- Create budget and send alarms when costs exceed the budget
- 4 types of budget: Usage, Cost, Reservation, Savings Plans
- For Reserved Instances
    - Track utilization
    - Supports EC2, ElastiCache, RDS, Redshift
- Up to 5 SNS notifications per budget
- Can filter by: Service, Linked Account, Tag, Purchase Option, Instance Type, Region, Availability Zone, API Operation, etc
- Same options and cost explorer

## Cost Anomaly Detection
- Continuously monitors your cost and usage using ML to detect unusual spends
- It learns your unique historical spend patterns to detect one-time cost spikes and/or continuous cost increases (you don't need to define thresholds)
- Monitor AWS services, member accounts, cost allocation tags, or cost categories
- Sends you the anomaly detection report with root-cause analysis
- Get notified with individual alerts or daily/weekly summary (using SNS)

## AWS Service Quotas
- Notify you when you're close to a service quota value threshold
- Example:
    - Getting close to the limit of how many Lambda functions you can run at the same time
- Create CloudWatch Alarms on the Service Quotas console
- Request a quota increase from the AWS Service Quotas or shutdown resources before limit is reached

## AWS Trusted Advisor
- No need to install anything - high level AWS account assessment
- Analyze your AWS accounts and provides recommendations in 6 areas:
    - Cost Optimization
    - Performance
    - Security
    - Fault tolerance
    - Service Limits
    - Operational Excellence
- Business and Enterprise Support plan is required for full set of checks
    - Programmatic Access using AWS Support API

## Support Plans for AWS
- Basic Support: free
- Business Support Plus: minimum $29/month per account
- Enterprise Support: minimum $5k per month
- Unified Operations: minimum $50k per month

### Basic Support
- Customer Service and Communities
    - 24/7 access to customer service, documentation, whitepapers, and support forums
- AWS Trusted Advisor - Access to the 7 core Trusted Advisor checks and guidance to provision your resources following best practices to increase performance and improve security 
- AWS Personal Health Dashboard - personalized view of the health of AWS services, and alerts when your resources are impacted

### AWS Business Support Plus
- Intended to be used if you have production workloads
- Real time and contextual responses through Generative AI
- Trusted Advisor - Full set of checks plus api access
- 24/7 phone, web, and chat access to Cloud Support Engineers
- Unlimited cases /unlimited contacts
- Max 30 minutes of waiting before getting a human support response for business-critical system down cases
- 3rd party software support (EC@ operating system like ubuntu)

### AWS Enterprise Support Plan
- All of Business Support+ Plan
- Intended to be used if you have production or business critical workloads
- Access to a designated Technical Account Manager (TAM)
- Less than 15 minutes for case response on production-critical issues
- Access to AWS Security Incident Response team
- Business reviews from AWS experts
- Access to AWS countdown event management (specialized TAM-led support to help you succeed during critical business events)

### AWS Unified Operations Support Plan
- Intended to be used if you have mission critical workloads
- All of Business Support Plan
- Application Architecture Guidance - helps you design architectures that fit your use case, workloads, etc
- Short-term engagement with AWS Support for deep understanding, analysis, then provide architectural guidance
- Access to a designated:
    - TAM (Technical Account Manager)
    - DSE (Domain Specialist Engineer)
    - SBAS (Senior Billing and Account Specialist)
    - IME (Incident Management Engineer)
    - Migration Specialist
    - SSE (Specialist Support Engineer)
- Access to AWS Countdown Premium and AWS Customer Incident Response Team (CIRT)
- Critical workload review

## Summary

### Account Best Practices
- Operate multiple accounts using organizations
- Use SCP (service control policies) to restrict account power
- Easily setup multiple accounts with best-practices with AWS Control Tower
- Use Tags and Cost Allocation Tags for easy management and billing
- IAM guidelines: MFA, least-privilege, password policy, password rotation
- Config to record all resources configurations and compliance over time
- CloudFormation: to deploy stacks across accounts and regions
- Trusted Advisor to get insights, Support Plan adapted to your needs
- Send Service Logs and Access Logs to S3 or CloudWatch Logs
- CloudTrail to record API calls made within your account
- If your Account is compromised: change the root password, delete and rotate all passwords / keys, contact the AWS support
- Allow users to create pre-defined stacks defined by admins using AWS Service Catalog

### Billing
- Compute Optimizer: recommends resources' configurations to reduce cost
- Pricing Calculator: estimate cost of services on AWS
- Billing Dashboard: high level overview of billing
- Cost Allocation Tags: tag resources to create detailed reports
- Cost and Usage Reports: most comprehensive billing dataset
- Cost Explorer: View current usage (detailed) and forecast usage
- Billing Alarms: in us-east-1 - track overall and per-service billing
- Budgets: more advanced - track usage, costs, RI, and get alerts
- Savings Plans: easy way to save based on long-term usage of AWS
- Cost Anomaly Detection: detect unusual spends using Machine Learning
- Service Quotas: notify you when you're close to service quota threshold 