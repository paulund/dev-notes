# Module 2 - Compute in the Cloud

## Introduction to Amazon EC2

- Compute refers to processing power needed to run applications
- Compute in the cloud is available on-demand by provisioning and managing in the cloud
- EC2 instances can be quickly launched, scaled and terminated compared to on-perm
- Only pay for when the resource is running, will not be charged when the instance is stopped
- Can customise the cpu, memory and storage of the server
- Can choose windows/lunix servers
- Easy to vertical scale the instance
- Multi-tenancy allows multiple virtual machines to share resources on the same physical host, with isolation between them.

## Instance Types

- EC2 instances can be customised with varies combinations of CPU, memory, storage and networking capabilities

| Type      | Description                                              | Ideal For                      |
|-----------|----------------------------------------------------------|-------------------------------|
| General   | Balanced CPU, memory, and networking                     | Web servers, development      |
| Compute   | High CPU resources                                       | Batch processing, analytics   |
| Memory    | High memory resources                                    | Databases, in-memory caches   |
| Storage   | High, fast local storage                                 | Data warehousing, big data    |
| Accelerated| Hardware accelerators (GPU, FPGA)                       | Machine learning, graphics    |

## Provision AWS Resources

- Provision a AWS EC2 instance can be done via an api or the console
- Console is good for test environments. API is better for prod environments.
- API can be run from the AWS CLI
- AWS SDK via programming language

## Launching an Amazon EC2 Instance

- AMIs = Amazon Machine Image
- AMIs are pre-built virtual machine images that have the basic components for what is needed to start an instance
- AMIs can be purchase from the aws marketplace
- AMIs are preconfigured instance types and software configurations

## AWS EC2 Pricing

- On-demand, pay what you use.
- Usage commitment discounts can be applied. 72% discount.
- Reserved instances can have a 75% discount. Need to commit to usage to either 1 year or 3 year of predictable workloads.
- Spot instances, 90% discount. But AWS can reclaim the instance when needed.
- Dedicated hosts. Own the whole server. Full control over how the resources are used.

## Scaling Amazon EC2

- Scalability is how the system can handle increased load
- Can scale vertically by increasing the resources
- Can scale horizontally by adding another instance
- Scalability is the ability of of scaling up and down when needed
- Elasticity is the ability to automatically scale up or down in real time depending on needs
- It will use key scaling metrics to decide when to scale the instances
- AWS Cloudwatch can be used to monitor these metrics and scale when needed
- Scaling can be done dynamically or predictive scaling based on schedules

## Directing Traffic with Elastic Load Balancing

- ELB used to distribute traffic to any EC2 instance
- It will take incoming requests and send them to any instances that are ready to accept traffic
- Can be used for traffic management at a routing level
- All incoming requests to be sent in to the load balancer and this will forward requests to the backend instance

## Messaging and queuing

- Amazon SQS used for message queuing in AWS
- SNS uses the publish/subscribe pattern to distribute messages
- Publish a single message and multiple application can subscribe to this message
- Amazon SNS used for sending notifications
- EventBridge is used to queue up multiple events and forward to the correct services
-
