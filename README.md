# Scalable Web Application Infrastructure on AWS

## Table of Contents

1. [Overview](#overview)
2. [Architecture Diagram](#architecture-diagram)
3. [Component Breakdown](#component-breakdown)
4. [Networking and Security](#networking-and-security)
5. [Monitoring and Alerts](#monitoring-and-alerts)
6. [High Availability and Scalability](#high-availability-and-scalability)
7. [Planned Enhancements](#planned-enhancements)

---

## Overview

This document presents the design of a scalable, highly available, and secure web application infrastructure built on Amazon Web Services (AWS). The architecture is tailored for cloud-native applications that demand resilience, automatic scaling, and efficient operations.

### Key Features:

* EC2 instances as application servers
* Load balancing with an Application Load Balancer (ALB) across multiple Availability Zones (AZs)
* Auto Scaling Group (ASG) for dynamic instance scaling
* Amazon RDS (Multi-AZ) for high availability of the relational database
* Monitoring via Amazon CloudWatch
* Notifications through Amazon Simple Notification Service (SNS)

### Design Principles:

* **Resilience**: Tolerates failure of individual components without affecting availability
* **Scalability**: Automatically adapts to workload changes
* **Security**: Enforced via network segmentation and strict access rules
* **Operational Efficiency**: Enabled by automation, monitoring, and alerting

---

## Architecture Diagram

The diagram below illustrates the system’s architecture:

![Architecture Diagram](aws%20project.png)

---

## Component Breakdown

### Compute Layer

* **Amazon EC2**: Hosts the application; deployed across two AZs for high availability
* **Launch Template / Configuration**: Defines parameters for EC2 instance creation
* **Auto Scaling Group (ASG)**: Maintains the desired number of EC2 instances; scales based on metrics such as CPU utilization
* **Application Load Balancer (ALB)**: Routes incoming traffic to healthy EC2 instances and performs health checks

### Database Layer

* **Amazon RDS (Multi-AZ Deployment)**:

  * **Primary Instance**: Handles all write operations
  * **Standby Instance**: Located in a separate AZ; automatically promoted during failover
  * RDS is deployed in private subnets and only accessible from within the VPC

### Monitoring and Notifications

* **Amazon CloudWatch**: Collects and visualizes logs and performance metrics
* **CloudWatch Alarms**: Triggered when metrics exceed thresholds (e.g., CPU > 80%)
* **Amazon SNS**: Sends alerts via email or SMS when alarms are triggered

---

## Networking and Security

### Virtual Private Cloud (VPC)

* Provides isolated and customizable networking for the infrastructure
* Spans two Availability Zones for redundancy

### Subnets

* **Public Subnets**:

  * Host the ALB and EC2 instances
  * Linked to a route table with an Internet Gateway for public access

* **Private Subnets**:

  * Host the RDS database
  * Not directly accessible from the internet

### Route Tables

* **Public Route Table**: Directs outbound internet traffic via the Internet Gateway
* **Private Route Table**: Handles only internal VPC traffic

### Security Groups

* **EC2 Security Group**: Permits traffic from the ALB on HTTP/HTTPS
* **ALB Security Group**: Allows inbound internet traffic on HTTP/HTTPS
* **RDS Security Group**: Allows traffic only from EC2 instances on port 3306 (MySQL)

### Internet Gateway

* Enables internet connectivity for instances in public subnets

> **Note**: NAT Gateway is not used, as all EC2 instances are in public subnets.

---

## Monitoring and Alerts

### Key Metrics Monitored with CloudWatch

* **EC2 Instances**:

  * `CPUUtilization`
  * `NetworkIn` / `NetworkOut`
  * `StatusCheckFailed`

* **RDS Instances**:

  * `CPUUtilization`
  * `DatabaseConnections`
  * `FreeStorageSpace`

### Alerts and Notifications

* **CloudWatch Alarms**:

  * Trigger on high CPU usage, instance failure, or low DB storage
* **Amazon SNS**:

  * Sends alerts to the operations team via email or SMS
  * Can integrate with Lambda functions or automation tools

---

## High Availability and Scalability

* **Multi-AZ Deployment**: Spreads resources across two AZs for redundancy
* **Auto Scaling Group**: Dynamically adjusts the number of EC2 instances based on load
* **RDS Multi-AZ**: Provides seamless failover to standby in the event of primary failure
* **ALB**: Ensures traffic is only routed to healthy application servers

---

## Planned Enhancements

* **Infrastructure as Code (IaC)**:

  * Automate deployments using Terraform, AWS CloudFormation, or AWS CDK

* **Security Enhancements**:

  * Integrate AWS WAF for web application protection
  * Add AWS Shield for DDoS mitigation

* **Logging and Auditing**:

  * Enable AWS CloudTrail for tracking API activity
  * Implement centralized logging with CloudWatch Logs Insights or ELK stack

* **Cost Optimization**:

  * Utilize EC2 Spot Instances or Savings Plans
  * Follow Trusted Advisor recommendations

* **CI/CD Integration**:

  * Use AWS CodePipeline, CodeBuild, and CodeDeploy for full automation

* **Backup and Recovery**:

  * Enable automated RDS backups and manual snapshots
  * Regularly test disaster recovery procedures

---

Let me know if you'd like a PDF version or need this reformatted for a GitHub README, Confluence page, or LinkedIn article.
