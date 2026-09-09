# Section 14: Cloud Monitoring

## CloudWatch Metrics
- provides metrics for **every** service in AWS
- Metric is a variable to monitor (CPU Utilization, Network In, etc)
- Metrics have timestamps
- Multiple metrics can be displayed on a CloudWatch Dashboard
- Example: CloudWatch Billing Metric
    - Limited to one region
    - Represents the amount youve spent on AWS cloud
- Top Metrics
    - EC2 Instances: CPU Utilization, Status Checks, Network (not RAM)
        - metrics update every 5 minutes by defauls
        - options for more detailed monitoring (like every 1 minute) - more expensive
    - EBS volumes:
        - Disk read/writes
    - S3 buckets:
        - BucketSizeBytes, NumberOfObjects, AllRequests
    - Billing: 
        - Total Estimated Charge
    - Service limits:
        - how much youve been using a service API
    - Custom metrics
        - push your own custom metrics

## CloudWatch Alarms
- Alarms trigger and action when a metric threshold is reached
- Alarm actions examples:
    - Auto Scaling: increase or decrease EC2 instances "desired" count
    - EC2 actions: stop, terminate, reboot, or recover an EC2 instance
    - SNS notifications: send a notification into an SNS topic
- Various trigger options
    - sampling
    - %
    - max
    - min
    - etc.
- choose a time period to evaluate if alarms should be triggered (last 5 minutes, 10 minutes, etc)
- Example:
    - create a billing alarm on the CloudWatch Billing metric
    - notify and admin if costs exceed $10
- Alarm States: OK, INSUFFICIENT_DATA, ALARM
- Billing Metrics and Alarms only available in US-EAST-1 (n. virginia)

## CloudWatch Logs
- CloudWatch can collect logs from: 
    - Elastic Beanstalk
    - ECS
    - AWS Lambda
    - CloudTrail
    - CloudWatch log agent
    - Route53
- enabled real-time monitoring
- Adjustable CloudWatch log retention

### CloudWatch Logs for EC2
- By default, no logs from your EC2 instances will go to CloudWatch
- You must run a CloudWatch agent on EC2 to push the log files you want
- EC2 instance must have proper IAM permissions to send logs to CloudWatch
- Agents can also be installed on on-premise servers

## Amazon EventBridge
- formerly CloudWatch Events
- reacts to events happen within your AWS account
- Schedule: allows scheduling cron jobs
- Event Pattern: Event rules to react to a service doing something
- You can also receive events from AWS partners and custom applications:
    - Partners: Zendesk, datadog, etc.
    - Custom: application can send an event when a user registers an account
- Schema Registry: model event schema
- You can archive events (all or filtered) sent to an event bus (indefinitely or for a set period)
- Ability to replay archive events


## AWS CloudTrail
- Provides governance, compliance, and audits for your AWS account
- CloudTrail is enabled by default
- Get a history of events/ API calls made within your AWS account by:
    - Console
    - SDK
    - CLI
    - AWS Services
- Can put logs from CloudTrail into CloudWatch Logs or s3
- A trail can be applied to **ALL** Regions (default)
    - or a single region
- EXAM NOTE: Need to know who did something? use CloudTrail

## AWS X-Ray
- Pros:
    - Troubleshooting performance
    - Understand dependencies in a microservice architecture
    - Pinpoint service issues
    - Review request behavior
    - Find errors and exceptions
    - Are we meeting time SLA?
    - Where is throttling happening
    - Identify impacted users

## AWS Health Dashboard

### Service Health
- Global and public
- General data about AWS services, not tied into your account
- Shows all regions and all services health
- shows historical information for each day
- Has an RSS feed you can subscribe to
- Previously called AWS service Health Dashboard

### Your Account Health
- Private and connected to you account
- Displays data about the AWS service you are using
- Previously called AWS Personal Health Dashboard (PHD)
- provides alerts and remidiation guidance when AWS is experiencing events that may impact you

### Your organization Health
- similar to your account health, but can be configured to pull data from **ALL** of the accounts in your organization

## Cloud Monitoring Summary
- CloudWatch
    - Metrics: monitor the performance of AWS services and billing metrics
    - Alarms: automate notifications, perform EC2 action, notify to SNS based on metric
    - Logs: collect log files from EC2 instances, servers, Lambda functions, etc
    - Events (or EventBridge): react to events in AWS, or trigger a rule on a schedule
- CloudTrail: audit API call made within your AWS
- CloudTrail Insights: automated analysis of your CloudTrail Events
- X-Ray: trace requests made through your distributed applications
- AWS Health Dashboard: Status of all AWS services across all regions
- AWS Account Health Dashboard: AWS events that impact your infrastructure

