# Module 3 - Exploring Compute Services

## Introduction In Serverless Compute
With serverless computing, you run applications without managing the underlying infrastructure.

- Amazon Elastic Container Service (Amazon ECS)
- Amazon Elastic Kubernetes Service (Amazon EKS)
- AWS Elastic Beanstalk

Unmanaged services like EC2 you are responisble for OS, network and firewall.
Managed services like AWS ECS the OS, network and firewall AWS is responisble.
Fully managed services like AWS Lambda AWS is responisble for handling the infrastructure

## AWS Lambda

- A serverless compute service
- Allows you to run code without managing a server
- Function as a service
- Lambda will run functions based on a trigger
- Lambda can run code in response to an event without needing to provision a new server
- It will automatically manage the infrastructure and scaling
- You are only charged for the compute time used
- Serverless services will make sure that your application can scale when needed

## Containers and Orchestration on AWS

- Containers create a consistent and portable runtime environment
- Amazon Elastic Container Registry (Amazon ECR) is used to store, manage, and version container images.
- Amazon ECS and Amazon EKS orchestrate containers to deploy, scale, and manage applications.
- AWS Fargate runs containers without the need to provision or manage servers, it's serverless
- ECS - Fully managed service based on your parameters
- EKS - kubernetes managed service

## Additional Compute Services

- AWS Beanstalk streamlines environment provisioning and management.
- Beanstalk can save configuration to be reused again
- AWS Batch manages large-scale computing tasks and automatically adjusts resources based on demand
- Amazon Lightsail streamlines web application setup and management without the need for complex infrastructure.
- AWS Outposts extends AWS services to on-premises environments, supporting hybrid cloud architectures.
-
