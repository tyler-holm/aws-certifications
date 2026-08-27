# Section 10: Other Compute Services: ECS, Lambda, Batch, Lightsail

## Docker
- Software development platform to deploy apps and services 
- Apps are packaged in contaiers that can be run on any OS
- Apps in a container run the same way regardless of where they-re run
    - Any machine
    - No compatibility issues
    - Less work
    - Predictable behavior
    - Easy to maintain and deploy
    - Works with any language, any OS, any technology
- Scale containers up and down very quickly

## Amazon ECR (Elastic Container Registry)
- Private docker repository hosted on AWS
- Store private images to be run by ECS or Fargate


## ECS (Elastic Container Service)
- Launch Docker containers on AWS
- You must provision and maintain EC2 instances to run the docker containers
- AWS takes care of starting / stoping containers 
- Has integrations with the Application Load Balancer

## Fargate
- Launch Docker containers on AWS
- You DO NOT need to provision and maintain EC2 instances to run the docker containers
- Serverless offering
- AWS runs the containers for you based on CPU/RAM requirements

## Amazon EKS
- EKS (Elastic Kubernetes Service)
- Allows you to launch managed Kubernetes clusters on AWS
- Kubernetes is an open-source system for management, deployment, and scaling of containerized apps (Docker)
- Containers can be hosted on
    - EC2 Instances
    - Fargate (Serverless)
- Kubernetes is cloud-agnostic
    - Not an AWS specific server, can be used in any cloud

## Serverless Overview
- New paradigm in which developers no longer manage servers
- They just deploy code or functions
- Initially -> serverless == FaaS (Function as a Service)
- Serverless was pioneered by AWS Lambda,
- Serverless offerings for many services
    - Databases (ex. DynamoDB)
    - APIs/functions (ex. Lambda)
    - Storage (ex. Amazon S3)
    - Docker (ex. Fargate)
    - etc.
- Serverless services still technically run on a server, but the server is managed and provisioned by AWS
- Users don't ever see or interact wit the underlying server

## AWS Lambda
- Limited by time - short executions
- Run on-demand
- Serverless
- Easy pricing
    - pay per request and compute time
    - tends to be VERY CHEAP which makes it VERY POPULAR
    - free tier is VERY generous
        - 1,000,000 free AWS Lambda requests ($0.2 per 1 million requests thereafter)
        - 400,000GB (RAM) seconds of free compute time (After that $1.00 for 600,000 GB seconds)
- Integrated with the full AWS service suite
- Event-Driven: functions get invoked by AWS when needed
- Integrated with many programming languages
- Easy monitoring through AWS CloudWatch
- Easy to get more resources per function (up to 10GB of RAM)
- Increasing RAM will also improve CPU and networking
- Can run many languages 
    - Node.js
    - Python
    - JAva
    - C# (.NET Core) / Powershell
    - Ruby
    - Many more through the Custom Runtime API (community supported)
    - Lambda Container Image

### Lambda Container Image
- Containers implementing the Lambda Runtime API
- ECS / Fargate is preferred for running other Docker images

### Lambda Usage Examples
- Usage Example: Serverless thumbnail creation
    - Users upload an image to S3
    - Lambda creates a thumbnail from the image
    - Lambda uploads the newly created thumbnail to S3 (could do other things like pushing image metadata to a DB)
- Usage Example: Serverless cron job
    - Create a Lambda function to fetch data from an external source
    - Create an event trigger in CloudWatch
    - CloudWatch triggers the lambda function every hour

## Amazon API Gateway
- Example: building a serverless API
    - Create a Lambda function to get data from a DynamoDB
    - Create an endpoint in the API Gateway pointing to the Lambda function
    - When external clients hit the API gateway it will proxy the request to the Lambda function
- Fully managed service for developers to easily create, public, maintain, monitor, and secure APIs
- Serverless and Scalable
- Supports RESTful APIs and WebSocket APIs
- Support for security, user authentication, API throttling, API keys, monitoring, etc...
- EXAM NOTE: Serverless API = API Gateway + Lambda

## AWS Batch
- Fully managed batch processing at any scale
- Efficiently run 100,000s of computing batch jobs on AWS
- A "batch" job is a job with a start and an end (opposed to continuous)
- Batch will dynamically launch EC2 instances or Spot Instances
- AWS Batch provisions the right amount of compute / memory
- You submit or schedule batch jobs and AWS batch does the rest
- Batch jobs are defined as Docker images and run on ECS, EKS, or Fargate
- Helpful for cost optimization and focusing less on infrastructure
- Usage Example: Process user images uploaded to S3
    - User upload an image to S3 (this triggers a batch job)
    - Batch runs an instance to process the image
    - Processed object gets inserted back into S3 (or somewhere else)

## Batch vs Lambda
- Lambda
    - Time limit
    - limited runtimes
    - limited temporary disk space
    - serverless
- Batch
    - no time limit
    - any runtime you want as long as its packaged as a docker image
    - Rely on EBS / instance store for disk space
    - Relies on EC2 (can be managed by AWS)

## AWS Lightsail
- Simplified standalone offers
- Poor integration with other AWS services
- Virtual servers, storage, databases, networking
- Low and predictable pricing
- Simpler alternative to using EC2, RDS, ELB, EBS, Route 53
- Great for people with little cloud experience
- Can setup notification and monitoring of your lightsail resources
- Use cases:
    - Simple web application (has templates for LAMP, Nginx, MEAN, Node.js, etc.)
    - Websites (templates for Wordpress, Magento, Plesk, Joomla)
    - Dev / Test environment
- Has high availability but no auto-scaling, limited AWS integration

## Summary
- Docker: container technoolgy to run applications
- ECS: run Docker containers on EC2 instances
- Fargate: run Docker containers without provisioning infrastructure (serverless)
- ECR: Private docker images repository
- Batch: run batch jobs on AWS across managed EC2 instances (runs on top of ECS)
- Lightsail: predictable & low pricing for simple application & DB stacks
- Lambda is serveless, function as a service, seamless scaling, reactive
    - Lambda Billing:
        - By the time run x by the RAM provisioned
        - By the number of invocations
    - Language Support:
        - many programming languages except (arbitrary) docker
    - Invocation time: up to 15 minutes
    - Use cases: 
        - Create thumbnails for images uploaded onto S3
        - run serverless cron jobs
- API Gateway: expose Lambda functions as HTTP APIs




