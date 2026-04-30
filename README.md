## aws-ecs-troubleshooting-lab

### Project Overview

This project simulates a real-world cloud support scenario where a containerized LMS (Learning Management System) application deployed on AWS ECS Fargate experiences critical failures.

As a Cloud Support Engineer, the objective was to diagnose and resolve issues related to networking, IAM permissions, and load balancing.

---

## Architecture Diagram

![Architecture Diagram](images/architecture-diagram.gif)

---

## Technologies Used

* AWS ECS (Fargate)
* Application Load Balancer (ALB)
* Amazon ECR
* Amazon CloudWatch
* AWS IAM
* AWS VPC (Public & Private Subnets)

---

## Issues Simulated

* ECS tasks failing to start due to misconfigured IAM roles
* ALB health checks failing due to incorrect target group settings
* Security group misconfigurations blocking traffic
* Container image pull failures from ECR

---

## Troubleshooting Process

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

In this phase, I containerized a React-based LMS frontend application using Docker and published the image to Amazon Elastic Container Registry (ECR). This prepares the application for deployment using AWS ECS in the next phase.
### Steps Performed

#### Step 1: Create ECR Repository

Created a repository to store container images.

![ECR Repository](images/ecr-repository.png)

#### Dockerfile Overview

The application was containerized using a custom Dockerfile based on a lightweight Node.js image.

Key configurations include:

- Using `node:16-alpine` for a minimal base image  
- Installing dependencies before copying full source code to optimize Docker layer caching  
- Building the React application for production (`npm run build`)  
- Serving static files using a lightweight Node server (`serve`)  
- Exposing port 3000 for application access  

📸 Screenshot:  
![Dockerfile](images/dockerfile.png)

#### Step 2: Build and Push Docker Image

- Built the LMS application Docker image locally using a custom Dockerfile  
- Tagged the image with the Amazon ECR repository URI to prepare for cloud deployment  
- Authenticated Docker with AWS using the AWS CLI  
- Successfully pushed the container image to Amazon ECR  

This process enables AWS ECS Fargate to securely pull and run the application in a scalable, containerized environment.

![Docker Image](images/docker-build.png)

![Push Image](images/Container-Image-Pushed.png)

#### Step 3: Verify Image in Amazon ECR

- Navigated to the Amazon ECR repository (`edutech-lms-frontend`)
- Confirmed the container image was successfully pushed with the `latest` tag
- Verified image metadata including URI, size, and status

📸 Screenshot:
![ECR Image Verification](images/ecr-image-verified.png)

The successful presence of the image in ECR confirms that the container is ready to be deployed via AWS ECS.

##  ECS Deployment with AWS Fargate

### Overview

In this phase, I deployed the containerized LMS frontend application to AWS using Elastic Container Service (ECS) with Fargate. This serverless deployment model eliminates the need to manage infrastructure while enabling scalable and highly available container execution.

---

### Steps Performed

#### Step 1: Create ECS Cluster

- Created an ECS cluster (`EduTech-LMS-Cluster`)
- Selected AWS Fargate as the launch type (serverless infrastructure)

![ECS Cluster](images/ecs-cluster.png)

*Fargate removes the need to manage EC2 instances, simplifying deployment and scaling.*

#### Step 2: Create Task Definition

- Defined task configuration (`EduTech-LMS-Task`)
- Configured container to use ECR image
- Set resource allocation (CPU & memory)
- Configured port mapping (3000 → HTTP)

![Task Definition](images/task-definition.png)

*Task definitions define how containers run, including compute resources and networking.*

#### Step 3: Configure Application Load Balancer

- Created an internet-facing Application Load Balancer (`EduTech-LMS-ALB`)
- Configured target group (`EduTech-LMS-TG`)
- Set health check path (`/`)
- Associated ALB with public subnets
  
Application Load Balancer:
![Load Balancer](images/alb.png) 

Target Group:
![Target Group](images/target-group.png)

*The ALB distributes incoming traffic and performs health checks for high availability.*

#### Step 4: Create ECS Service

- Deployed service (`EduTech-LMS-Service`) using Fargate
- Linked service to task definition and load balancer
- Configured networking (VPC, subnets, security groups)
- Set desired task count to maintain availability

ECS Service:
![ECS Service](images/ecs-service.png)

*ECS Service ensures containers remain running and automatically integrates with load balancing.*

#### Step 5: Access Application

- Retrieved Application Load Balancer DNS name
- Accessed deployed LMS frontend via browser

![Live Application](images/live-app.gif)

## Key Outcomes

In this phase of the project, I successfully:

- Designed and provisioned an ECS cluster using AWS Fargate to support serverless container workloads  
- Defined and configured task definitions to control container runtime behavior and resource allocation  
- Implemented an Application Load Balancer to distribute incoming traffic and enable high availability  
- Deployed a containerized LMS frontend application, making it accessible through a public endpoint  

This phase demonstrates the end-to-end deployment of a cloud-native application using AWS-managed container services.

## ECS Troubleshooting: From Symptoms to Solutions

### Overview

In this phase, I simulated a container failure by introducing an invalid health check to observe how ECS handles unhealthy tasks and recovers from errors.

Issue Introduced (Task Revision 3)

### Configured an invalid health check:

CMD-SHELL, curl -f http://localhost:3000/non-existent-path || exit 1

Failure Evidence:

![Live Application](images/Bad-Health-Check.png)

![Live Application](images/Bad-Health-Check2.png)

![Live Application](images/Bad-Health-Check3.png)

### Diagnosis
- Tasks failed health checks and were marked UNHEALTHY
- ECS Events tab showed repeated “failed container health checks” messages
- Tasks were marked UNHEALTHY and moved to STOPPED state
- ECS continuously stopped and restarted containers

### Resolution

Updated the health check to a valid endpoint and created a new task revision:

CMD-SHELL, curl -f http://localhost:3000/ || exit 1

### Service Recovery

![Healthy Service](images/Healthy-Service.png)

- Updated task definition with a valid health check endpoint  
- Redeployed ECS service using the latest revision  
- ECS successfully replaced failing tasks and restored service stability  

## Troubleshooting ALB Health Check Misconfiguration

### Overview

In this phase, I simulated an Application Load Balancer (ALB) misconfiguration to understand how incorrect health check settings impact service availability and traffic routing.

### Issue Introduced (Health Check Port Misconfiguration)

The target group health check was intentionally configured with an incorrect port:
- Health Check Port: 3001 (incorrect)
- Application Port: 3000 (actual)

![Misconfigured Health Check](images/Misconfigured-Health-Check.png)

### Observing the Failure

After applying the misconfiguration, I monitored the target group:
- Targets transitioned from Healthy → Unhealthy
- Health checks failed consistently
- Errors indicated connection timeout / refused connection

 ![Unhealthy-Target](images/Unhealthy-Target.png)


 ![Health-Check-Errors](images/Health-Check-Errors.png)

Using the AWS Console, I identified the root cause:
- ALB health checks were targeting the wrong port
- The application was running on port 3000, but ALB checked 3001
- This caused all targets to fail health checks

Additionally:

- Healthy host count dropped to zero
- ALB stopped routing traffic entirely

Impact:
- Application became inaccessible via the load balancer
- No healthy targets available to serve requests
- Simulated a real-world outage caused by configuration error

### Resolution: Fixing the Health Check

To restore service, I corrected the health check configuration:

Health Check Port: traffic-port
This ensures the ALB checks the correct port that the application is listening on.

![Fixed Health Check Configuration](images/Fixed-Health-Check-Configuration.png)
 
### Recovery Monitoring

After applying the fix:
- Targets gradually transitioned back to Healthy
- Health checks began passing successfully
- ALB resumed normal traffic routing

![Healthy Targets](images/Healthy-Targets.png)

Result
- Service was fully restored
- Targets stabilized in a Healthy state
- Application became accessible again via the load balancer

### Summary
This exercise demonstrated how a simple ALB health check misconfiguration can disrupt service availability and how to systematically diagnose and resolve the issue using AWS tools.

## Security Group Configuration Lab: Diagnosing and Fixing ALB Access Issues
  
### Overview
In this phase, I simulated a security group misconfiguration to understand how network-level access controls impact Application Load Balancer (ALB) accessibility. By blocking all inbound traffic, I was able to observe how improper firewall rules can make an application completely unreachable.

### Issue Introduced: Misconfigured Security Group

A new security group was created with no inbound rules, effectively blocking all incoming traffic:

- Security Group: EduTech-Misconfigured-SG
- Inbound Rules: None (all traffic blocked)
- Outbound Rules: Allow all

This configuration prevents any external requests from reaching the ALB.

![sg misconfigured](images/sg-misconfigured.png)

Observing the Impact
After applying the misconfigured security group to the ALB:

- Application became inaccessible via browser
- Requests resulted in timeout / connection errors
- No inbound traffic reached the load balancer

![alb timeout](images/alb-timeout-error.png)

![Alb Unhealthly Checks](images/alb-UnhealthlyChecks.png)

### Diagnosis
Investigation revealed:
- The ALB security group had no inbound rules
- Port 80/443 traffic was completely blocked
- External users could not establish a connection to the ALB

Additionally:
- ALB remained running, but unreachable
- Target group health may remain stable since internal checks can still pass

Impact:
- Complete loss of external access to the application
- Simulated a real-world outage caused by firewall misconfiguration
- Demonstrated how network controls directly affect availability

### Resolution: Fixing Security Group Rules

To restore access, I reconfigured the ALB security group:
Inbound Rules:
HTTP (80) → 0.0.0.0/0
HTTPS (443) → 0.0.0.0/0

Alternatively, I reattached the original ALB security group with proper rules.

![Fixed sg settings](images/sg-fixed.png)

### Recovery Verification

After applying the fix:
- Application became accessible again via ALB DNS
- Requests successfully reached the load balancer
- Service returned to normal operation

![ALB Working](images/alb-working.png)

Result:
- ALB successfully accepted incoming traffic
- Application accessibility fully restored
- Security group rules validated and corrected

Key Takeaways:
- Security groups act as critical network firewalls in AWS
- Missing inbound rules can completely block application access
- ALB can appear “healthy” but still be unreachable externally
- Proper port configuration (80/443) is essential for public services

### Summary:
This exercise demonstrated how a simple security group misconfiguration can block all inbound traffic and cause a full application outage. By identifying and correcting the firewall rules, normal service operation was restored.


## Project Summary
This project demonstrates the end-to-end deployment, troubleshooting, and management of a containerized application on AWS using modern cloud-native services.

Throughout the project, I:
- Containerized a Learning Management System (LMS) application using Docker
- Deployed the application on Amazon ECS Fargate with a scalable architecture
- Configured an Application Load Balancer (ALB) for traffic distribution
- Implemented networking and security controls using VPC and Security Groups
- Diagnosed and resolved real-world issues including:
-  Container health check failures
-  Resource constraints
-  ALB misconfigurations
-  Security group access issues
- Monitored system behavior using AWS service insights and logs
  
### Key Skills Demonstrated
- Containerization with Docker
- Cloud deployment using AWS ECS Fargate
- Load balancing and traffic routing (ALB)
- Network security and access control (Security Groups, VPC)
- Troubleshooting and debugging cloud infrastructure
- Observability using AWS monitoring tools

### Final Thoughts
This project provided hands-on experience with building and troubleshooting a production-style cloud environment. It highlights the importance of understanding how different AWS services interact and how small misconfigurations can impact system availability.

---
