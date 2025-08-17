# Manara-Project-AWS_SAA 
## Scalable Web Application with ALB and Auto Scaling (Manara AWS Graduation Project)

---

## Project Requirements (from Manara Guidelines)

> ### Project 1: Scalable Web Application with ALB and Auto Scaling  
> **Architecture:** EC2-based  
>  
> **Description:**  
> Deploy a simple web application on AWS using EC2 instances, ensuring high availability and scalability with Elastic Load Balancing (ALB) and Auto Scaling Groups (ASG). The project demonstrates best practices for compute scalability, security, and cost optimization.  
>  
> **Key AWS Services Used:**  
> - EC2: Launch instances for the web app.  
> - ALB: Distributes traffic across multiple instances.  
> - ASG: Ensures instances scale based on demand.  
> - Amazon RDS (Optional): Backend database with Multi-AZ.  
> - IAM: Role-based access to instances.  
> - CloudWatch & SNS: Monitor performance and send alerts. 
>  
> **Learning Outcomes:**  
> - Setting up secure and scalable EC2-based web applications  
> - Implementing high availability using ALB and ASG  
> - Optimizing costs and performance using Auto Scaling policies  

---

## Project Overview

This project demonstrates how to deploy a **highly available** and **scalable web application** on AWS using key infrastructure components such as **EC2**, **Application Load Balancer (ALB)**, and **Auto Scaling Groups (ASG)**. It follows AWS best practices to ensure performance, reliability, security, and cost-efficiency.

---

## Solution Architecture Diagram

![AWS_SAA-diagram](https://raw.githubusercontent.com/ahmed323salama/Manara-Project-AWS_SAA/refs/heads/main/AWS_SAA-diagram.png)

This diagram visualizes the complete setup: a multi-tier architecture across 2 Availability Zones, public and private subnets, ALB, ASGs, NAT Gateways, RDS, and monitoring services.

---

## AWS Services Used

- **Amazon EC2** – Hosts the web and application servers
- **Application Load Balancer (ALB)** – Balances traffic between EC2 instances
- **Auto Scaling Group (ASG)** – Automatically adjusts EC2 capacity
- **Amazon RDS** – Relational database (MySQL/PostgreSQL) in Multi-AZ
- **IAM** – Secures access between services
- **Amazon CloudWatch** – Collects and tracks metrics
- **Amazon SNS** – Sends alerts and notifications
- **Amazon Route 53** – DNS service to route users to the application
- **NAT Gateway & Internet Gateway** – Provide controlled internet access to public and private resources
- **Amazon VPC** – Isolated network with subnets and routing
- **Security Groups** – Virtual firewalls controlling inbound and outbound traffic for EC2, RDS, and ALB
- **Route Tables** – Define how traffic is routed between subnets, NAT Gateways, and Internet Gateways

---

### Deployment Setup

#### 1. **VPC & Subnets**
- Create a custom VPC with CIDR (e.g., `172.16.0.0/16`)
- Create 2 public subnets and 4 private subnets across **2 Availability Zones**

#### 2. **Internet & NAT Gateways**
- Attach an **Internet Gateway** to the VPC  
- Add **1 NAT Gateway per AZ** in public subnets  
- Update route tables to allow:
  - Public subnets → Internet Gateway
  - Private subnets → NAT Gateway  

#### 3. **Security Groups**
- Create:
  - `WEB Security Group`: Allow HTTP/HTTPS from ALB  
  - `APP Security Group`: Allow traffic from WEB SG  
  - `DB Security Group`: Allow traffic from APP SG  

#### 4. **EC2 Instances**
- Launch EC2 Instances:
  - Web Layer in Public Subnets (for static content or load testing)
  - Application Layer in Private Subnets behind ALB  
- Attach proper **IAM Roles** for EC2 access to CloudWatch.

#### 5. **Application Load Balancer**
- Configure ALB across 2 AZs  
- Register web EC2 instances with ALB target group  
- ALB forwards traffic to instances via listeners (HTTP/HTTPS)

#### 6. **Auto Scaling Group (ASG)**
- Create launch template/launch configuration  
- Define scaling policies (based on CPU utilization or target tracking)  
- Distribute instances across AZs for fault tolerance  

#### 7. **Amazon RDS**
- Deploy RDS (MySQL/PostgreSQL) in **Multi-AZ** with private subnet group  
- Connect RDS to EC2 app instances using DB Security Group  

#### 8. **Monitoring & Alerts**
- Enable CloudWatch metrics and logs for EC2, ALB, and RDS  
- Create CloudWatch alarms ( CPU > 80%)  
- Set up SNS topics to send email alerts to administrators

#### 9. **DNS Setup with Route 53**
- Create a hosted zone  
- Point domain or subdomain to the ALB DNS name

