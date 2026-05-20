# NestJS AWS ECS Deployment
Containerized NestJS application deployed on AWS ECS Fargate using Docker and Amazon ECR.


# Architecture

```txt
Developer
   ↓
Docker Build
   ↓
Amazon ECR
   ↓
Amazon ECS Fargate
   ↓
Public API Endpoint
```

# Technologies
* NestJS
* TypeScript
* Docker
* AWS ECS
* AWS ECR
* AWS IAM
* AWS CloudWatch

# Docker
## What is Docker
Docker is a containerization platform used to package applications and dependencies into portable containers

## Dockerfile
```dockerfile
FROM node:20

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

RUN npm run build

EXPOSE 3000

CMD ["node", "dist/main"]
```

## Docker Commands
### Build Image

```bash
docker build -t nestjs-app .
```

### Run Container

```bash
docker run -p 3001:3000 nestjs-app
```

# AWS ECR
## What is ECR
Elastic Container Registry is a Docker image registry service provided by AWS.
Used for storing Docker images before deployment

## ECR Workflow

```txt
Docker Image
    ↓
Push to ECR
    ↓
Deploy to ECS
```

## ECR Login
```bash
aws ecr get-login-password --region ap-southeast-2 | docker login --username AWS --password-stdin <ECR_URL>
```

## Push Docker Image
### Tag Image

```bash
docker tag nestjs-app:latest <ECR_URL>/nestjs-app:latest
```

### Push Image

```bash
docker push <ECR_URL>/nestjs-app:latest
```

# AWS ECS
## What is ECS
Elastic Container Service is a container orchestration service for running Docker containers on AWS

## ECS Responsibilities
* Container deployment
* Container management
* Networking
* Scaling
* Service availability

# AWS Fargate
## What is Fargate
AWS Fargate is a serverless compute engine for containers.
AWS manages the infrastructure automatically without requiring EC2 server management.

## Why Fargate
* No server management
* Easy deployment
* Scalable
* Cost efficient for small workloads

# ECS Task Definition
## What is Task Definition
Task Definition is a blueprint that defines how the container runs

## Task Definition Configuration
Configuration  | Value     
---------------------------
Launch Type    | Fargate   
CPU            | 0.25 vCPU 
Memory         | 0.5 GB    
Container Port | 3000      

## Why 0.25 vCPU and 0.5 GB
The NestJS API is lightweight and intended for low traffic workloads
Using smaller resources helps optimize AWS infrastructure cost

# IAM
## What is IAM
Identity and Access Management manages permissions and access to AWS services

## IAM Concepts Learned
* IAM Users
* Access Keys
* Policies
* Service Linked Roles

## Policies Used
* AdministratorAccess
* AmazonEC2ContainerRegistryFullAccess

# CloudWatch
## What is CloudWatch
AWS CloudWatch is a monitoring and logging service

## Used For
* ECS logs
* Container monitoring
* Debugging deployment issues

# Real Issues and Troubleshooting

# 1. Docker Daemon Not Running
## Error

```txt
failed to connect to the docker API
```

## Solution
Started Docker Desktop manually

# 2. Dockerfile Not Found
## Error

```txt
failed to read dockerfile
```

## Cause
Docker build command executed in the wrong directory

## Solution

```bash
cd backend
docker build -t nestjs-app .
```

# 3. Port Already In Use
## Error

```txt
bind: Only one usage of each socket address
```

## Cause
Port 3000 already occupied by another process

## Solution

```bash
docker run -p 3001:3000 nestjs-app
```

# 4. ECR Permission Denied
## Error

```txt
not authorized to perform: ecr:GetAuthorizationToken
```

## Cause
IAM user lacked ECR permissions

## Solution
Attached:
* AmazonEC2ContainerRegistryFullAccess

# 5. ECR Repository Does Not Exist
## Error

```txt
repository does not exist
```

## Solution
Created ECR repository manually

# 6. ECS Service Linked Role Error
## Error

```txt
Unable to assume the service linked role
```

## Cause
Missing ECS service-linked role

## Solution
Created ECS service-linked role using IAM

# 7. CloudFormation Stack Already Exists
## Error

```txt
A CloudFormation stack already exists
```

## Cause
Previous failed ECS cluster stack still existed

## Solution
Deleted failed CloudFormation stack or created a new cluster name

# Final Result
Successfully deployed a containerized NestJS application to AWS ECS Fargate with public internet access.

# Skills Demonstrated
* Docker Containerization
* AWS ECS Deployment
* AWS ECR
* IAM Permission Management
* Cloud Troubleshooting
* Container Networking
* Cloud Architecture

# Future Improvements
* GitHub Actions CI/CD
* Load Balancer
* Auto Scaling
* HTTPS
* Terraform
* ECS Blue/Green Deployment

# Test
Public IP : 13.54.75.219:3000

# Author
spnk8
