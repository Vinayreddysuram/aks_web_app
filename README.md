# Sample HTML Web Application on Azure

This project demonstrates how to deploy a sample HTML web application using Azure Kubernetes Service (AKS) and Azure Container Registry (ACR).

## Overview

The application serves a simple HTML page and is deployed in Azure using a containerized approach with Docker.

## Azure Resources

- **Resource Group Name**: `assignment2`
- **Azure Container Registry Name**: `htmlwebappace`
- **Azure Kubernetes Service Name**: `htmlwebapp`
- **Location**: `Canada Central`

## Prerequisites

Before you begin, ensure you have the following installed:

- [Azure CLI](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli)
- [Docker](https://www.docker.com/products/docker-desktop/)
- [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
- [Helm](https://helm.sh/docs/intro/install/)



### 1. Set Up Your Local Environment

Make sure your terminal/command prompt is open and ready.

### 2. Create the Sample HTML Application

### 3. Steps for deploying on AKS


####  1. Build the Docker Image
Build the Docker image locally:

docker build -t sample-web-app .

####  5. Run the Docker Container Locally
Run the Docker container to test locally:

docker run -d -p 8080:80 sample-web-app

Open your browser and navigate to http://localhost:8080 to verify the application is running.

#### 6. Log in to Azure
Log in to your Azure account:

az login

#### 7. Create the Azure Container Registry (ACR)
Create a resource group:

az group create --name assignment2 --location "Canada Central"

Create the Azure Container Registry:


az acr create --resource-group assignment2 --name htmlwebappace --sku Basic

#### 8. Log in to ACR
Log in to your ACR instance:

az acr login --name htmlwebappace


#### 9. Tag and Push the Docker Image to ACR

Tag the Docker image:

docker tag sample-web-app htmlwebappace.azurecr.io/sample-web-app:v1

Push the Docker image to ACR:

docker push htmlwebappace.azurecr.io/sample-web-app:v1

#### 10. Create the Azure Kubernetes Service (AKS) Cluster
Create the AKS cluster:

az aks create --resource-group assignment2 --name htmlwebapp --node-count 1 --enable-addons monitoring --generate-ssh-keys

#### 11. Get AKS Credentials
Get the credentials to access your AKS cluster:

az aks get-credentials --resource-group assignment2 --name htmlwebapp

#### 12. Connect AKS to ACR
Attach the ACR to your AKS cluster:


az aks update --name htmlwebapp --resource-group assignment2 --attach-acr htmlwebappace

#### 13. Create Kubernetes Deployment and Service Manifests
Create deployment.yaml:
Create service.yaml:

#### 14. Apply the Kubernetes Manifests
Deploy the application using the Kubernetes manifests:

kubectl apply -f deployment.yaml
kubectl apply -f service.yaml


#### 15. Get the External IP of Your Service
Check the status of your service to get the external IP:

kubectl get service sample-web-app-service

Note the external IP address. It might take a few minutes to be assigned.

#### 16. Access the Web Application
Open your browser and navigate to the external IP address to view your sample HTML web application.

#### Few this to consider for securing the AKS cluster and website

1. While creating the AKS cluster, we can set authorized IP addresses to access the API server. This means that if an IP address is not included, even if a user logs in with their Azure account using the CLI, they will not be able to access the AKS cluster

2. While creating the Elastic Load Balancer using the service.yaml file, we can use annotations to restrict access to specific IP addresses, effectively acting as a firewall.