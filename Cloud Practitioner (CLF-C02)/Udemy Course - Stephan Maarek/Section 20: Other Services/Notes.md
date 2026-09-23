# Section 20: Other Services

# Amazon Workspaces
- Managed Desktop as a Service (DaaS) solution to easily provision Windows or Linux Desktops
- Great to eliminate management of on-premise VDI (virtual desktop infrastructure)
- Fast and quickly scales to thousands of users
- Secured data - integrates with KMS
- Pay-as-you-go service with monthly or hourly rates
- Multiple Regions
    - Deploy workspace as close as possible to users

## Amazon AppStream 2.0
- Desktop Application Streaming Service
- Deliver to any computer, without acquiring, or provisioning infrastructure
- Application is delivered from within a web browser
- instance type can be configured per application

## AWS IoT Core
- allows you to easily connect IoT devices to the AWS Cloud
- Serverless, secure, and scalable to billions of devices and trillions of messages
- Applications can communicate with your devices even when they aren't connected
- Integrates with a lot of AWS services 
- Build IoT applications that gather, process, analyze, and act on data

## AWS AppSync
- Store and sync data across mobile and we apps in real-time
- Makes use of GraphQL (mobile technology from Facebook)
- Client code can be generated automatically
- Integrations with DynamoDB / Lambda
- Teal-time subscriptions
- Offline data synchronization (replaces Cognito Sync)
- Fine Grained Security
- AWS Amplify can leverage AWS AppSync in the background


## AWS Amplify
- a set of tools and services that helps you develop and deploy scalable full stack web and mobile applications
- Authentication, Storage, API (Rest, GraphQL), CI/CD, PubSub, Analytics, AI/ML Predictions, Monitoring, Source Code from AWS, GitHub, etc.

## AWS Infrastructure Composer
- Visually design and build serverless applications quickly on AWS
- Deploy AWS Infrastructure code without needing to be an expert in AWS
- Configure how your resources interact with each other
- Generates Infrastructure as Code (IaC) using CloudFormation
- Ability to import existing CloudFormation / SAM templates to visualize them

## AWS Device Farm
- fully managed service that tests your web and mobile apps against desktop browsers, real mobile devices, and tablets
- Run tests concurrently on multiple devices
- Ability to configure device settings (GPS, language, WiFi, Bluetooth)
- can interact with devices directly
- access to reports, logs, screenshots

## AWS Backup
- Fully managed service to centrally manage and automate backups across AWS services
- On-demand and scheduled backups
- Supports PITR (Point in time recovery)
- Retention Periods, Lifecycle Management, Backup Policies
- Cross-Region Backup
- Cross-Account Backup (using AWS Organizations)

## AWS Elastic Disaster Recovery (DRS)
- Quickly and easily recover your physical, virtual, and cloud-based servers into AWS
- Example:
    - protect your most critical databases, protect enterprise apps, protect your data from ransomware attacks, etc
    - Continuous block-level replication for your servers
    - AWS Replication agent will replicate all your data into the AWS cloud with EBS volumes 
    - Can create failover production servers in minutes from the replicated volumes
    - when the main system is back online you can failback to the original servers

## Cloud Migration Strategies: the 7Rs
- Retire
    - turn off things you don't need
    - Helps with reducing the attack surface area
    - Save costs
    - Focus attention on resources that must be maintained
- Retain 
    - Don't migrate for now
    - Maybe for security, data compliance, performance, unresolved dependencies
    - No business value to migrate, mainframe or mid-range and non-x86 Unix apps
- Relocate
    - Move apps from on-premise to its Cloud version
    - Move EC2 instances to a different VPC, AWS account, or AWS Region
    - Example: transfer servers from VMware Software-defined Data Center (SSDC) to VMware Cloud on AWS
- Rehost ("lift and shift") 
    - Simple migrations by re-hosting on AWS (applications, database, data, etc)
    - Migrate machines (physical, virtual, another Cloud) to AWS
    - No cloud optimizations being done, application is migrated as is
    - Could save as much as 30% on costs
    - Example: Migrate using AWS Application Migration Service
- Replatform  ("lift and reshape)
    - Example: migrate your database to RDS
    - Example: migrate your application to Elastic Beanstalk
    - not changeing the core architecture, but leverage some Cloud optimizations
    - Save time and money by moving to a fully managed service or Serverless
- Repurchase ("drop and shop")
    - Moving to a different product while moving to the Cloud
    - Often you move to a SaaS platform
    - Expensive in the short term, but quick to deploy
    - Example: CRM to Salesforce.com, HR to workday, CMS to Drupal
- Refactor/Re-architect
    - Reimagining how the application is architected using Cloud Native features
    - Driven by the need of the business to add features and improve scalability, performance, security, and agility
    - Move from a monolithic application to micro-services
    - Example: move an application to Serverless architectures, use AWS S3

## AWS Application Discovery Service
- Plan migration projects by gathering information about on-premises data centers
- Server utilization data and dependency mapping are important for migrations
- Agentless Discovery (AWS Agentless Discovery Connector)
    - VM inventory, configuration, and performance history
- Agent-based Discovery (AWS Application Discovery Agent)
    - System configuration, system performance, running processes, and details of the network connections between systems
- Resulting data can be viewed within AWS Migration Hub

## AWS Application Migration Service (MGN)
- Lift-and-Shift (rehost) solution which simplifies migrating applications to AWS
- Converts your physical, virtual, and cloud-based servers to run natively on AWS
- supports wide range of platforms, operating systems, and databases
- minimal downtime, reduced costs

## AWS Migration Evaluator
- Helps you build a data-driven business case for migration to AWS
- Provides a clear baseline of what your organization is running today
- Install Agentless Collector to conduct broad-based discovery
- Take a snapshot of on-premise foot-print, server dependencies
- Analyze current state, define target state, then develop migration plan

## AWS Migration Hub
- Central location to collect servers and application inventory data for the assessment, planning, and tracking of migrations to AWS
- Helps accelerate your migration to AWS, automate lift-and-shift
- AWS Migration Hub Orchestrator - provides pre-built templates to save time and effort migrating enterprise apps
- Supports migration status updates from Application Migration Service (MGN) and database Migration Service (DMS)

## AWS Fault Injection Simulator (FIS)
- Fully managed service for running fault injection experiments on AWS workloads
- Based on Chaos Engineering - stressing and application by creating disruptive events (sudden CPU or memory increase), observing how the system responds, and implementing improvements
- Help you uncover hidden bug and performance bottlenecks
- Supports the following AWS services: EC2, ECS, EKS, RDS, etc
- Uses pre-defined templates that generate the desired disruptions

## AWS Step Functions
- Build serverless visual workflow to orchestrate your Lambda functions
- Feature: sequence, parallel, conditions, timeouts, error handling, etc
- Can integrate with EC2, ECS, On-premises servers, API Gateways, SQS Queues, etc
- Possibility of implementing human approval feature
- Use cases: order fulfillment, data processing, web application, any workflow

## AWS Ground Station
- Fully managed service that lets you control satellite communications, process data, and scale your satellite operations
- Provides a global network of satellite ground stations near AWS regions
- Allows you to download satellite data to your AWS VPC within seconds
- Send satellite data to S3 or EC2 instance
- Use cases: weather forecasting, surface imaging, communications, video broadcasts

## AWS Pinpoint
- Scalable 2-way (outbound/inbound) marketing communications service
- Supports email, SMS, push, voice, and in-app messaging
- Ability to segment and personalize message wit the right content to customers
- Possibility to receive replies 
- Scales to billions of messages per day
- Use cases: run campaigns by sending marketing, bulk, transactional SMS messages
- Versus Amazon SNS or Amazon SES
    - In SNS & SES you manage each message's audience, content, and delivery schedule
    - In amazon pinpoint, you create message templates, delivery schedules, highly-targeted segments, and full campaigns