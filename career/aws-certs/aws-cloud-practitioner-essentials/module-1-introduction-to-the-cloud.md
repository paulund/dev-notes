# Module 1 - Introduction To The Cloud

## What Is Cloud Computing
Cloud computing is the on-demand delivery of IT resources with pay as you go
pricing.

### Deployment types
- Cloud resources
- On-perm deployment
- Hybrid cloud and on-perm solution

## Benefits Of AWS Cloud
- Pay as you go pricing
- Benefit from economies of scale
- Dynamically scaling to meet your resource needs
- Increase speed and agility
- No need to run and maintain a data centre
- Quicker to market


## AWS Regions
Regions are locations around the world that contain groups of data centers.
These groups are called availability zones, each regions has a minimum of
3 availability zones.

## AWS Availability Zone
An availability zone consists of one or more data center with redundent power,
networking and connectivity.

Availability zones consist of isolation resources, this way you can distribute resources across multiple availability zones giving you great availability if one zone was compromised.

For example if you had 2 VMs for your application behind a load balancer then it
would be a good idea to put the VMs in different availability zones.

## The AWS Shared Responsibility Model
Both the customer and AWS are responisble for securing the application.

Customer responisbility
- Customer data
- Client side data encryption

AWS & Customer responisbility
- Serverside data encryption
- Network traffic protection
- Platform, applications, identify and access management
- Operating system, network and firewall configuration

AWS Responisbility
- Compute, storage, database, network updates
- Hardware, Global infrastructure

## Cloud In Real Life
- Use regions close to your customer
- Faster and cheaper to get to market
- Higher availability by using multiple zones
