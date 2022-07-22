---
title: "Project 4-MLops pipeline on AWS"
excerpt: "MLops on AWS<br/><img src='/images/proj4.png'>"
collection: projects
---

As the Senior Solutions Architect, you have have been tasked with optimizing the data transfer to more cost-effective solutions. You have identified the cost savings of moving a MySQL database currently running on an EC2 instance to an RDS Aurora MySQL instance, and have decided to use the AWS Data Migration Service. Let's get started!

Ensure you are in us-east-1 and log in to the AWS console using the credentials provided.

Note: The lab uses an RDS MySQL instance target to stand in for an Aurora DB to speed the lab's setup for you. The migration process is the same.

# Project overview


## Step1. Deploy an EC2 instance

Option1: Click create EC2 instance
Option2: Use the preconfigured template called Amazon Machine Image(AMI) where you specify the OS
<img width="1185" alt="image" src="https://user-images.githubusercontent.com/44923423/180217914-6a6797d9-9ad4-4041-ac28-b2af42b5492a.png">

**Why wanna use EC2?**
- Deploy DB on EC2 can give you the full control over the DB
- Deploy website on EC2 on multiple regions can make it highly available

**Methods to access EC2 instance**
- AWS Management Console: simple stuff just use this
- **Secure Shell(SSH)**: establish a secure connection to your instances from your local laptop
- EC2 Instance Connect (EIC): control SSH using IAM policies, no need to manage SSH keys
- AWS System Manager: allows you to manage your EC2 instances via a web browser or the AWS CLI

The most common way to connect to Linux EC2 instances is via secure shell(SSH)
(1) Generate a key pair
SSH client on laptop uses the private key, and the EC2 instance uses the public key
(2) Connect to your EC2 instance via SSH

**Price**
- On-demand: no contract, charge by seconds that you use, when your application is less than 1 year
- Spot(cheapest option): save up to 90% of the on-demand price, but work might be interrupted
- Researved instances(RIS): need to sign contract for 1-3 years


## Container
<img width="971" alt="image" src="https://user-images.githubusercontent.com/44923423/180221357-5fd4a481-70f2-41ad-bb51-7e2ad9c95aec.png">
<img width="1052" alt="image" src="https://user-images.githubusercontent.com/44923423/180221778-3c9f7fb2-6609-4113-b9de-ed6eda26d99f.png">
<img width="1052" alt="image" src="https://user-images.githubusercontent.com/44923423/180221971-207c2b84-6992-4a58-91d6-ee0842d6285c.png">
<img width="1052" alt="image" src="https://user-images.githubusercontent.com/44923423/180222203-b7c06ef9-5509-4af8-b048-919f3faf47d1.png">
<img width="1052" alt="image" src="https://user-images.githubusercontent.com/44923423/180222343-ecfe1641-b10f-4a96-888f-9f90bf89dfc1.png">


## Storage

- S3: 
<img width="1126" alt="image" src="https://user-images.githubusercontent.com/44923423/180222581-4fe2d383-ba2f-49ae-9152-5f473f68e52f.png">
<img width="1126" alt="image" src="https://user-images.githubusercontent.com/44923423/180222816-74c1ea19-4240-4008-9194-fd7b247543fa.png">
<img width="1126" alt="image" src="https://user-images.githubusercontent.com/44923423/180222939-440ac424-5bd7-4ece-87dd-c3dcb8b872a9.png">

**Which model to choose?**
<img width="1276" alt="image" src="https://user-images.githubusercontent.com/44923423/180223090-e8379cd3-eb15-48b6-82de-fa061cd860a4.png">
<img width="1276" alt="image" src="https://user-images.githubusercontent.com/44923423/180223309-00822897-04a6-45de-bada-b4e74bf4f4ac.png">
<img width="1276" alt="image" src="https://user-images.githubusercontent.com/44923423/180223500-61657e5f-f6f0-4a10-bd62-e2a50376e071.png">
<img width="1276" alt="image" src="https://user-images.githubusercontent.com/44923423/180223782-fb1453a5-4522-43e1-a3ab-8872031a5e02.png">
<img width="1289" alt="image" src="https://user-images.githubusercontent.com/44923423/180224039-82731c30-a120-4b13-87e0-0623bd6d574f.png">

# Terms:
## Region/Avaliable Zoon/data center
- Region > AZ > data center > servers > virtual instances (not considered serverless)



