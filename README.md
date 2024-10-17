# Game Application Deployment on Amazon EKS with Fargate

This repository contains the end-to-end deployment of a game application on an Amazon Elastic Kubernetes Service (EKS) cluster using AWS Fargate. The deployment utilizes Kubernetes services such as auto-scaling, auto-healing, service discovery, and load balancing to ensure high availability and scalability of the application.

## Table of Contents

- [Introduction](#introduction)
- [Prerequisites](#prerequisites)
- [Architecture](#architecture)
- [Deployment Overview](#deployment-overview)
- [Steps to Deploy](#steps-to-deploy)
  - [Step 1: Create an EKS Cluster](#step-1-create-an-eks-cluster)
  - [Step 2: Deploy the Game Application](#step-2-deploy-the-game-application)
  - [Step 3: Service Discovery and Load Balancing](#step-3-service-discovery-and-load-balancing)
- [Testing the Deployment](#testing-the-deployment)
- [Conclusion](#conclusion)
- [License](#license)

## Introduction

This project demonstrates the deployment of a game application on Amazon EKS using Fargate for serverless Kubernetes. The application is deployed as containers, managed by Kubernetes, and utilizes replicas for auto-healing and scaling. It is exposed to external users through an Application Load Balancer (ALB) to distribute traffic efficiently and ensure reliable access.

## Prerequisites

Before you begin, ensure you have the following:

- **AWS CLI** configured on your local machine.
- **kubectl** installed and configured to interact with EKS.
- **eksctl** for easy cluster management.
- **Docker** to build and push container images.
- **AWS account** with sufficient permissions to create EKS clusters, IAM roles, and security groups.

## Architecture

The deployment includes:

- **Amazon EKS Cluster** with Fargate for serverless container management.
- **Kubernetes Deployments** for managing application replicas.
- **Application Load Balancer (ALB)** for traffic distribution.
- **Auto-Scaling and Auto-Healing** capabilities using Kubernetes.
- **External IP exposure** for public access to the game application.

## Deployment Overview

1. **Cluster Provisioning**: Create an Amazon EKS cluster with Fargate profiles.
2. **Containerization**: Build Docker images of the game application and push them to a container registry (Amazon ECR or Docker Hub).
3. **Kubernetes Deployment**: Deploy the game application using Kubernetes manifests, ensuring auto-scaling and healing through replicas.
4. **Service Discovery**: Use Kubernetes services for internal routing and discovery.
5. **Load Balancing**: Configure an ALB for distributing traffic to the application.

## Steps to Deploy

### Step 1: Create an EKS Cluster

Use `eksctl` to create an EKS cluster with Fargate profiles.

```bash
eksctl create cluster \
  --name game-cluster \
  --version 1.21 \
  --region us-west-2 \
  --fargate
```
## Step 2: Deploy the Game Application
Build Docker Image:
Build the Docker image for the game application and push it to your container registry (e.g., Amazon ECR or Docker Hub).

## bash
Copy code
-docker build -t your-game-app .\
-docker tag your-game-app:latest <your-repo-uri>/your-game-app:latest.\
-docker push <your-repo-uri>/your-game-app:latest.\
-Deploy to EKS:\
-Use the provided Kubernetes manifest files (deployment.yaml and service.yaml) to deploy the application to the EKS cluster.\

## bash
Copy code --
kubectl apply -f deployment.yaml\
kubectl apply -f service.yaml\
Ensure that the deployment creates replicas for auto-scaling and healing.\

## Step 3: Service Discovery and Load Balancing\
Configure an Application Load Balancer (ALB):\
To expose the application to the outside world, configure a Kubernetes Service of type LoadBalancer.\

## yaml Code
Copy code --
apiVersion: v1
kind: Service
metadata:
  name: game-app-service
spec:
  selector:
    app: game-app
  type: LoadBalancer
  ports:
    - port: 80
      targetPort: 8080

## Expose External IP:
Once the service is created, an external IP address will be assigned by AWS. You can use this IP to access the application.

## bash
Copy code --
kubectl get services
Testing the Deployment
Once the game application is successfully deployed, you can access it through the external IP or DNS name provided by the load balancer. Open your web browser and navigate to the address to verify the application is running as expected.

## bash
Copy code --
kubectl get svc game-app-service
Access the game application using the provided external IP.

## Conclusion
This deployment demonstrates the power of Amazon EKS with Fargate for running containerized applications in a serverless environment. With Kubernetes managing replicas and load balancing via ALB, the application is scalable, reliable, and easy to maintain.

## License
This project is licensed under the MIT License - see the LICENSE file for details.
