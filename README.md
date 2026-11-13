# Secure Container Deployment on AWS ECS Fargate

## Project Overview

This project demonstrates how to securely deploy a Dockerized Python application using Amazon ECS with AWS Fargate.

The application retrieves database credentials from AWS Secrets Manager and sends application logs to Amazon CloudWatch.

The deployment was completed as part of a hands-on Cloud Computing & DevOps lab at Montreal College of Information Technology (MCIT).

## Architecture

**Docker → Amazon ECR → Amazon ECS Fargate → Amazon CloudWatch**

AWS Secrets Manager securely provides credentials to the running container through an environment variable.

## AWS Services Used

- **Docker:** Builds and packages the Python application.
- **Amazon ECR:** Stores the Docker container image.
- **Amazon ECS:** Manages the container deployment.
- **AWS Fargate:** Runs containers without managing EC2 servers.
- **AWS IAM:** Controls permissions through task and execution roles.
- **AWS Secrets Manager:** Stores and injects database credentials.
- **Amazon CloudWatch:** Collects application logs.

## Implementation

### 1. Docker Image and Amazon ECR

- Cloned the provided application repository.
- Reviewed the Python application and Dockerfile.
- Built the Docker image using AWS CloudShell.
- Created a private Amazon ECR repository.
- Tagged and pushed the Docker image to Amazon ECR.

### 2. IAM Roles and Secrets Manager

- Created `ecsTaskExecutionRole` with the required execution permissions.
- Added a policy allowing access to AWS Secrets Manager.
- Created `ecsTaskRole` for the application.
- Configured a database credentials secret in AWS Secrets Manager.

### 3. ECS Fargate Deployment

- Created an ECS Fargate task definition.
- Configured CPU, memory, IAM roles, and container image.
- Injected database credentials using the `DB_CREDENTIALS` environment variable.
- Deployed the application using an ECS service.
- Configured VPC networking and public IP assignment.

### 4. Monitoring and Verification

- Verified that the ECS task was running successfully.
- Confirmed successful secret injection.
- Reviewed application output in CloudWatch Logs.
- Verified that the application heartbeat was working.

## Screenshots

Evidence collected during the lab:

1. Amazon ECR repository and Docker image.
2. IAM roles and permissions.
3. ECS Fargate task definition.
4. ECS service with one running task.
5. Application logs confirming successful secret injection.

## Results

**The deployment was successful.**

The Docker container ran on AWS Fargate, received its database credentials from AWS Secrets Manager, and generated logs visible in Amazon CloudWatch.

## What I Learned

- How to build and push Docker images to Amazon ECR.
- How ECS and Fargate work together.
- The difference between ECS task roles and execution roles.
- How to securely inject secrets into containers.
- How to monitor containerized applications using CloudWatch.
- How to troubleshoot deployment and configuration issues.

## Environment

- **Cloud Provider:** AWS
- **Region:** us-east-1 (N. Virginia)
- **Application:** Python
- **Container Technology:** Docker
- **Compute:** Amazon ECS Fargate

---

**Author:** Sebastian Vita  
**Program:** Cloud Computing & DevOps — MCIT  
**Project Type:** Hands-on AWS Lab
