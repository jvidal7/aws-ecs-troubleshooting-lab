## aws-ecs-troubleshooting-lab

### 📌 Project Overview

This project simulates a real-world cloud support scenario where a containerized LMS (Learning Management System) application deployed on AWS ECS Fargate experiences critical failures.

As a Cloud Support Engineer, the objective was to diagnose and resolve issues related to networking, IAM permissions, and load balancing.

---

## 🏗️ Architecture Diagram

![Architecture Diagram](images/architecture-diagram.gif)

---

## 🛠️ Technologies Used

* AWS ECS (Fargate)
* Application Load Balancer (ALB)
* Amazon ECR
* Amazon CloudWatch
* AWS IAM
* AWS VPC (Public & Private Subnets)

---

## 🚨 Issues Simulated

* ECS tasks failing to start due to misconfigured IAM roles
* ALB health checks failing due to incorrect target group settings
* Security group misconfigurations blocking traffic
* Container image pull failures from ECR

---

## 🔍 Troubleshooting Process

1. Checked ECS task logs using CloudWatch
2. Identified IAM permission issues preventing container startup
3. Verified ALB target group and health check configuration
4. Fixed security group rules to allow proper traffic flow
5. Validated container image availability in ECR

---
## Environment Setup (AWS Infrastructure)

## Overview

This section outlines the setup of the AWS environment required to host a containerized Learning Management System (LMS). It includes IAM roles, networking (VPC), and security group configurations necessary for a secure and scalable deployment.

Prerequisites
AWS account with administrative access
Basic understanding of AWS networking concepts
Access to AWS Console

Step 1: AWS Account & Region Setup
Logged into AWS Console
Selected region: us-east-1

![Selected Region](images/Selected-region.png)

Step 2: IAM Role Configuration

Created two IAM roles for ECS:

1. ECS Service Role
 * Service: Elastic Container Service
 * Role Name: EduTech-ECS-Service-Role

2. ECS Task Execution Role
 * Role Name: EduTech-ECS-Task-Role
 * Policies attached:
   * AmazonECSTaskExecutionRolePolicy
   * CloudWatchLogsFullAccess
     
EduTech-ECS-Service-Role:

![EduTech-ECS-Service-Roles-Creation](images/EduTech-ECS-Service-Role-Creation.png)

EduTech-ECS-Task-Role:

![Edu-ECS-Task-Role-Creation-and-Roles](images/EduTech-ECS-Task-Role-Creation-and-Roles.png)


Step 3: VPC Network Setup

Created a custom VPC:

* Name: EduTech-VPC
* CIDR: 10.0.0.0/16
* Availability Zones: 2
* Public Subnets: 2
* Private Subnets: 0 (lab simplification)
* NAT Gateway: None

![VPC-createion-page](images/VPC-creation-page.png)

Step 4: Security Groups Configuration
ALB Security Group (EduTech-ALB-SG)
HTTP (80) → 0.0.0.0/0
HTTPS (443) → 0.0.0.0/0
Container Security Group (EduTech-Container-SG)
Port 3000 → Only from ALB security group


ALB security group rules:

![ALB Security Group Rules Creation](images/ALB-security-group-rules-Creation.png)

Container security group rules:

![Container security group rules](images/Container-security-group-rules.png)

Step 5: Resource Documentation

Recorded key resource identifiers:

VPC ID & Subnet IDs:
  
![VPC ID and Subnet](images/VPC-ID-from-VPC-Dashboard.png)

Security Group IDs:

![Security Group IDs](images/Security-group-IDs-from-VPC-Dashboard.png)

IAM Role ARNs:

![IAM Role ARNs](images/IAM-role-ARNs-from-IAM.png)

## Container Deployment & ECS Setup

### Overview

In this phase, I containerized the LMS application and deployed it using AWS ECS Fargate. This included setting up an ECR repository, defining task configurations, and running the service behind a load balancer.

### Steps Performed

#### Step 1: Create ECR Repository

Created a repository to store container images.

![ECR Repository](images/ecr-repository.png)
---

## ✅ Outcome

* Successfully deployed and stabilized the LMS application
* Resolved networking, IAM, and load balancing issues
* Improved understanding of AWS container-based architectures

---

## 📚 Key Learnings

* Importance of IAM roles in ECS task execution
* Debugging using CloudWatch logs
* Understanding ALB routing and health checks
* Real-world troubleshooting mindset in cloud environments
