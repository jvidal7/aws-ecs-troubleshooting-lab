# aws-ecs-troubleshooting-lab

## 📌 Project Overview

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
⚙️ Environment Setup (AWS Infrastructure)

Overview
This section outlines the setup of the AWS environment required to host a containerized Learning Management System (LMS). It includes IAM roles, networking (VPC), and security group configurations necessary for a secure and scalable deployment.

Prerequisites
AWS account with administrative access
Basic understanding of AWS networking concepts
Access to AWS Console

Step 1: AWS Account & Region Setup
Logged into AWS Console
Selected region: us-east-1

![image alt]images/Selected region.png
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
