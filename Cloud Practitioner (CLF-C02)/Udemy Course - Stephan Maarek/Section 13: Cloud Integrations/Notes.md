# Section 13: Cloud Integrations
 
## Patterns of application communications
- Synchronous communications (application to application)
    - Bi-directional 
    - Can cause problems when there are sudden spikes in traffic
    - (Service 1) <----> (Service 2) 
- Asynchronous / Event based (application to queue to application)
    - uni-directional
    - handles sudden traffic spikes much better
    - (Service 1) -----> (Queue) -----> (Service 2) 

## Amazon SQS
- Asynchronous application communication service
- "Producers" push messages into the SQS Queue
- "Consumers" Poll the queue for messages
- Messages are deleted from queue after they are processed by consumers
- Oldest AWS offering (over 10 years old)
- Serverless
- Fully managed service, use to decouple applications
- Scales from 1 message per second to 10,000s per second
- Default message retention: 4 days
- Maximum retention: 14 days
- No limit to how many message can be in the queue
- Low latency
- Consumers share the work and scale horizontally
- Standard Queue
    - Ordering is not preserved
- FIFO Queue
    - First in first out

## Kinesis
- EXAM NOTE: Kinesis = real-time big data streaming
- Asynchronous application communication service
- Managed service to collect, process, and analyze real-time streaming data at any scale

## SNS
- Asynchronous application communication service
- For sending one message to many receivers
- Pub/Sub integration
- Publishers only send messages to one SNS topic
- Subscribers listen to the SNS topic for notifications
- no message retention
- Each subscriber will get all the messages
- up to 12,500,000 subscriptions per topic
- 100,000 topics maximum limit

## Amazon MQ
- managed message broker service for rabbitmq and activemq
- Doesn't scale as well as SQS/SNS
- runs on servers, can run in Multi-AZ with failover
- has a queue feature and a topic feature
- Use case: 
    - applications using traditional on-premise style queues 
    - applications using open protocols like MQTT, AMQP, STOMP, Openwire, WSS