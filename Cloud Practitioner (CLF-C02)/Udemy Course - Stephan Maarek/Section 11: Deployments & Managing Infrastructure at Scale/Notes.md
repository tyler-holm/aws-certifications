# Section 11: Deployments & Managing Infrastructure at Scale

NOTE: This is the most interesting section so far.

## CloudFormation
- Declarative way of outlinig your AWS Infrastructure, for any resource
- Example CloudFormation Template:
    - I want a ecurity group
    - I want two EC2 instances using this security group
    - I want an S3 bucket
    - I want a load balancer (ELB) in front of all of these machines
    - Then CloudFormation creates these for you, in the right order, with the exact configuration that you specify

### Benefits of CloudFormation
- Infrastructure as code
    - No resources are created manually, which is excellent for control
    - Changes to the infrastructure are reviews through code
- Cost
    - Each resource within the stack is tagged with an identifier so you can easily see how much a stack costs you
    - You can estimate the costs of your resources using the CloudFormation template
    - Savings startegy: In Dev, you could automate deletion of templates at 5PM and recreate at 8AM, safely
- Productivity
    - Ability to destroy and re-create an infrastructure on the cloud on the fly
    - Automated generation of Diagrams for you templates
        - Diagrams include all the resources
        - You can see all the relations between the components
    - Declarative programming (no need to figure out ordering and orchestration)
- Don't re-invent the wheel
    - Leverage existing templates on the web
    - Leverage existing documentation
- Support (almost) all AWS resources
    - Can use a "custom resource" for resources that aren't supported

## AWS Cloud Development Kit (CDK)
- Define your cloud infrastructure using a familiar language:
    - JavaScript/TypeScripts, Python, JAva, and .NET
- Code is converted into a CloudFormation template (JSON/YAML)
- Allows deploying infrastructure and application runtime code together
    - Great for Lambda functions
    - Great for Docker Containers in ECS/EKS


## Elastic Beanstalk

### Overview 
- Platform as a service (PaaS)
- developer centric view of deploying an application on AWS
- uses all the same components you'd use if configuring manually
- all components are condensed into one easy to understand view
- still have full control over the configuration
- Managed service
    - Instance configuration / OS is handled by Beanstalk
    - Deployment strategy is configurable but performed by Elastic Beanstalk
    - Capacity provisioning
    - Load balancing and auto-scaling
    - Application health-monitoring & responsiveness
- Support for many platforms
    - Go
    - Java SE
    - Java with Tomcat
    - .NET on Windows Server with IIS
    - Node.js
    - PHP
    - Python
    - Ruby
    - Packer Builder
    - Docker
    - and more

### Problems solved by Beanstalk
- Managing infrastructure
- Deploying Code
- Configuring all the databases, load balancers, etc
- Scaling Concerns
- Most web apps have the same architecture (ALB + ASG)
- All developers want is for their code to run consistently, they don't care about these problems
- Developers are only responsive for application code

### Architectuire Models
- Single instance deployment
    - good for dev
- LB + ASG 
    - great for production of pre-production web applications
- ASG only:
    - great for non-web apps in production (workers, etc..)

### Health Monitoring
- Health agent pushes metrics to CloudWatch
- Checks for app health, publishes health events

## CodeDeploy
- deploy application updates automatically from a single interface
- Works with EC2 instances
- Works with on premise servers
- Hybrid service
- servers / instances must be provisioned and configured ahead of time with the CodeDeploy Agent

## CodeCommit
- AWS alternative to GitHub
- Version controlled Git-based repository
- code changes are automatically versioned 
- Benefits:
    - Fully managed
    - Scalable and highly available
    - Private, Secured, Integrated with AWS

## CodeBuild
- Cloud based code building service
- Compiles source code, run tests, produces packages that are ready to be deployed (by CodeDeploy for example)
- Comparable to a gitlab runner
- Benefits:
    - Fully managed, serverless
    - Continuously scalable and highly available
    - Secure
    - Pay-as-you-go pricing - only for build time

## CodePipeline
- Orchestrates the different Code steps to have the code automatically deployed to production
- Basis for CICD (Continuous Integration and Continuous Delivery)
- Benefits:
    - Fully managed
    - compatible with CodeCommit, CodeBuild, CodeDeploy, Elastic Beanstalk, CloudFormation, GitHub, 3rd-party services & custom plugins, many more
    - Fast delivery and rapid updates

## CodeArtifact
- Handles the storage and retrieval of software dependencies
- Secure, stable, and cost-effective
- Works with common dependency management tools
    - Maven
    - Gradle
    - npm
    - yarn
    - pip
    - and many more
- Developers and CodeBuild can retrieve dependencies straight from CodeArtifact






