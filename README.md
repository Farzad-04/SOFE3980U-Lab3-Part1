# Lab 3 Part 1: Deploying using Google Kubernetes Engine

## Overview
This lab focuses on deploying a web application using Google Kubernetes Engine (GKE). The process includes containerizing the application using Docker, deploying a MySQL server, and hosting the Maven-based Binary Calculator WebApp on GKE. YAML files are used to simplify the deployment process.

## Table of Contents
1. Creating GCP Account
2. Setting up Google Kubernetes Engine (GKE)
3. Deploying MySQL Server on GKE
4. Deployment using YAML Files
5. Deploying the Maven Project
6. Discussion
7. Design
8. Deliverables

## Objectives
- Get familiar with Docker and Kubernetes.
- Use Google Cloud Platform (GCP).
- Deploy a Maven WebApp on Google Kubernetes Engine (GKE).

## Repository
[GitHub Repository](https://github.com/zubxxr/SOFE3980U-Lab3-Part1)

## Technologies Used
- **Docker**: Containerization of the web application.
- **Kubernetes**: Orchestrating containers in GKE.
- **Google Cloud Platform (GCP)**: Hosting the Kubernetes cluster.
- **MySQL**: Database deployment.
- **Maven**: Project build management.

## Deployment Steps

### 1. Setting Up GKE
- Set the default compute zone:
  ```sh
  gcloud config set compute/zone northamerica-northeast1-b
  ```
- Enable Kubernetes Engine API:
  ```sh
  gcloud services enable container.googleapis.com
  ```
- Create a three-node cluster:
  ```sh
  gcloud container clusters create sofe3980u --num-nodes=3
  ```

### 2. Deploying MySQL Server on GKE
- Deploy MySQL using `kubectl`:
  ```sh
  kubectl create deployment mysql-deployment --image mysql/mysql-server --port=3306
  ```
- Verify deployment:
  ```sh
  kubectl get deployments
  ```
- Retrieve logs and access MySQL:
  ```sh
  kubectl logs <pod-name> | grep GENERATED
  kubectl exec -it <pod-name> -- mysql -uroot -p
  ```
- Create a service for MySQL:
  ```sh
  kubectl expose deployment mysql-deployment --type=LoadBalancer --name=mysql-service
  ```

### 3. Deploying MySQL Using YAML
- Deploy MySQL with YAML files:
  ```sh
  cd ~/SOFE3980U-Lab3-Part1/MySQL
  kubectl create -f mysql-deploy.yaml
  kubectl create -f mysql-service.yaml
  ```

### 4. Deploying the Maven Project
- Build the project:
  ```sh
  cd ~/SOFE3980U-Lab3-Part1/BinaryCalculatorWebapp
  mvn package
  ```
- Build and push Docker image:
  ```sh
  docker build -t <repo-path>/binarycalculator .
  docker push <repo-path>/binarycalculator
  ```
- Deploy the application:
  ```sh
  kubectl create deployment binarycalculator-deployment --image <repo-path>/binarycalculator --port=8080
  kubectl expose deployment binarycalculator-deployment --type=LoadBalancer --name=binarycalculator-service
  ```
- Retrieve the external IP and access the application:
  ```sh
  kubectl get service
  ```
  Open `http://<IP-address>:8080` in a browser.

## Discussion
- **Docker & Kubernetes**: Containerization and orchestration technologies that simplify deployment and scalability.
- **Advantages of Docker**:
  - Portability
  - Consistency across environments
  - Efficient resource usage
- **Disadvantages**:
  - Learning curve
  - Additional security considerations

## Design
- Update the Binary Calculator Application with the latest version.
- Replace manual deployments with YAML files.
- Implement redeployment using YAML.

## Deliverables
- A report covering the discussion and deployment steps.
- GitHub repository with the updated application and YAML files.
- Two audible videos:
  1. **MySQL Deployment** (5 minutes)
  2. **Binary Calculator Deployment** (3 minutes)
- Screenshots showing application deployment and execution.

## Conclusion
This lab provided hands-on experience in deploying a Maven-based web application using Docker and Kubernetes on GKE. It demonstrated cloud-based container orchestration, database integration, and automated deployment using YAML.

--
