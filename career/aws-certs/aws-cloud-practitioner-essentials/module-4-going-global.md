# Going Global

Going into more details of the AWS infrastructure for regions and availability zones.

- How to choose a region
- Value of edge locations
- AWS CloudFormation to streamline deployment
- How to achieve high availability

## Introduction to Going Global

- Select region based on local demand, regulations, latency and cost.
- Edge locations will cache resources closer to users, improving access speed.
- AWS CloudFormation for IAC (Infrastructure as Code)


## Choosing an AWS Region

- Where are your customers located?
- Are there any local regulations or compliance requirements?
- What is the latency to your customers?
- What are the costs associated with each region?
- Compliance, proximity, features and price

## Diving Deeper into AWS Global Infrastructure

- Using multiple regions will increase high availability. If one region goes
down, you can failover to another region.
- Edge locations will cache content closer to users, improving access speed. This will
use AWS CloudFront.
- Edge locations are separate from AWS regions and are located in major cities around the world.
- AWS Route 53 is a scalable and highly available Domain Name System (DNS) web service that can route users to the nearest edge location.
- Regions are physical locations where AWS has data centers.
- Availability Zones (AZs) are isolated locations within a region.

## Infrastructure And Automation

- For high availability you can use 2 different regions, taking advantage of
infrastructure as code will allow you to automate the deployment and management of resources across these regions.
- Use AWS CloudFormation to define and provision your infrastructure as code.
- Create templates that can be reused in multiple environments and regions.
