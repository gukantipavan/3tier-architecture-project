# 3-Tier High Availability Architecture on AWS

## Overview
This project demonstrates the implementation of a **3-Tier Architecture** using AWS services like:
- **Elastic Load Balancer (ALB)**
- **Auto Scaling Groups (ASG)**
- **Multi-AZ RDS (MySQL)**
- **Public and Private Subnets** for the separation of the web, application, and database layers.

The architecture ensures **high availability, fault tolerance, and scalability**.

![Architecture Diagram](~/Downloads/3tierarchitecture/3tier.webp)

---

## Table of Contents
- [Overview](#overview)
- [Architecture Diagram](#architecture-diagram)
- [Prerequisites](#prerequisites)
- [AWS Services Used](#aws-services-used)
- [Implementation Steps](#implementation-steps)
- [Test and Validate](#test-and-validate)
- [Conclusion](#conclusion)

---

## Architecture Diagram
Here is the visual representation of the 3-tier architecture:

![3-Tier Architecture Diagram](~/Downloads/3tierarchitecture/3tier.webp)

---

## Prerequisites
Before starting, ensure you have the following:
- An AWS account.
- Basic understanding of AWS VPC, EC2, ALB, Auto Scaling, and RDS.
- AWS CLI (optional) if you want to automate parts of the deployment.

---

## AWS Services Used
- **VPC**: Custom VPC with public and private subnets.
- **Internet Gateway (IGW)**: To allow internet access to public subnets.
- **Route Tables**: For routing internet traffic to public subnets and isolating private subnets.
- **EC2 Instances**: Used for the application layer.
- **ALB (Application Load Balancer)**: Distributes traffic to instances in multiple AZs.
- **Auto Scaling Group**: Automatically adjusts the number of EC2 instances in response to demand.
- **RDS (MySQL)**: Multi-AZ database for high availability.
- **Security Groups**: Control inbound and outbound traffic at the instance level.

---

## Implementation Steps

### Step 1: Create a Custom VPC
- **Create VPC**: CIDR block `10.0.0.0/16`.
- **Subnets**: Create 2 public subnets and 4 private subnets across 2 AZs.
- **Internet Gateway**: Attach an IGW to the VPC.
- **Route Tables**: 
  - Public Subnet Route Table (routes internet traffic).
  - Private Subnet Route Table (isolated).

### Step 2: Launch EC2 Instances with Auto Scaling
- **Create a Launch Template** for EC2 instances (use `t2.micro` for free tier).
- **Configure Auto Scaling Groups** with desired capacity of 2 instances.
- **Attach Security Groups**:
  - Allow inbound HTTP (80) and SSH (22) for public subnets.
  - Allow inbound traffic from the ALB to private instances (port 80).

### Step 3: Set Up Application Load Balancer (ALB)
- **Create an ALB** in public subnets.
- **Configure Target Group** to route traffic to EC2 instances in private subnets.
- **Health Checks**: Set health checks on the target group using HTTP and path `/`.

### Step 4: Deploy RDS (MySQL) in Multi-AZ
- **Create RDS** in private subnets.
- **Multi-AZ Deployment**: Enable this for failover support.
- **Security Group**: Allow traffic only from the application layer.

### Step 5: Configure Route 53 (Optional)
- **DNS Configuration**: Configure Route 53 for custom domain (if applicable).

---

## Test and Validate
- Access the **Load Balancer DNS** (found in EC2 > Load Balancers) to test the deployed application.
- Check the **Auto Scaling** feature by simulating high traffic or terminating an EC2 instance.
- Verify **RDS failover** by manually forcing a failover in the RDS settings.

---

## Conclusion
This project demonstrates how to build a scalable, fault-tolerant, and highly available architecture on AWS using the 3-tier model. It leverages the power of AWS services such as Auto Scaling, ALB, and RDS to provide high availability and fault tolerance.

---

## Future Improvements
- Implement HTTPS with SSL/TLS for security.
- Add CloudFront for content delivery.
- Automate the deployment using AWS CloudFormation or Terraform.

