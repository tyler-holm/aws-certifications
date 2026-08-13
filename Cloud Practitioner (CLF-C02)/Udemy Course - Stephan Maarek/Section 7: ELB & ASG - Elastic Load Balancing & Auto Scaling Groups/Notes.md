# Section 7: ELB & ASG - Elastic Load Balancing & Auto Scaling Groups

## Scalability & High Availablility

- Scalability: the ability to adapted a system or application to handle greater loads by scaling up (increasing hardware), or scaling out (adding nodes)
- Types of Scalability:
    - Vertical Scalability
    - Horizontal Scalability (elasticity)
- Scalability is linked to, but different from high availability

### Vertical Scalability
- increasing the size of an instance
- example: increasing the computing power of an EC2 instances

### Horizontal Scalability
- increasing the number of instances running
- example: adding another EC2 instance behind a load balancer to handle more traffic

### High Availability
- Goes hand in hand with horizontal scaling
- High availability means running your application / system in at least 2 availability zones
- Goal is to survive if a datacenter goes down

### Elasticity
- auto-scaling so that the system can scale dynamically based on the load. 
- Pay-per use, match demand, optimize costs

### Agility (distractor)
- resources are only a click away, which means that you reduce the time to make those resources available to your developers from weeks to minutes

## Elastic Load Balancing (ELB)

### Overview

- Load balancers are servers that forward internet traffic to multiple servers (EC2 Instances) downstream
- Expose a single access point (DNS) and the load balancer handles which server gets used
- Seamlessly handles failures of downstream instances (redundancy)
- Provides SSL termination (HTTPS) for your websites
- Can be used across multiple availability zones

### Benefits of ELB
- managed load balancer
- AWS guarantees that it will be working
- AWS takes care of upgrade, maintenance, high availability
- Only requires a few configuration knobs

- It costs less to setup your own load balancer, but it will be a lot more effort on your end.

### Types of AWS Load Balancers
- [ALB] Application Load Balancer 
    - Layer 7
    - HTTP/HTTPS/gRPC
    - HTTP Routing features
    - Static DNS (URL)
- [NLB] Network Load Balancer 
    - Layer 4
    - TCP/UDP protocols
    - Ultra-high performance
    - Static IP through elastic IP
- [GWLB] Gateway Load Balancer
    - Layer 3
    - GENEVE protocol
    - route traffic to Firewalls that you manage on EC2 instances
    - intrusion detection / packet inspection
    - traffic goes to GWLB, to a 3rd party security application, back to the GWLB, and then on to the destination application
- Classic Load Balancer 
    - Layer 4 & 7
    - retired 2023 (replaced by ALB and NLB)

## Auto Scaling Group (ASG)

### Goal of ASG
- Scale out (add EC2 instances) to match an increased load
- Scale in (remove EC2 instances) to match a decreased load
- Automatically register new instances to a load balancer
- Detect and replace unhealthy instances
- Cost savings: only run at optimal capacity
