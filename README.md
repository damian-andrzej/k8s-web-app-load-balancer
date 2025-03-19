# k8s-web-app-load-balancer
Cloud K8s cluster setup and application deployment exposed to internet by load balancer

# Deploy Web App to DigitalOcean Kubernetes with Load Balancer

## Overview

This repository demonstrates how to deploy a **web application** to a **DigitalOcean Kubernetes (DO K8s)** cluster and expose it to external traffic using a **Load Balancer**. It uses a simple **Flask** app (Python) as the example application, but you can adapt the template to any web application framework.

---

## Prerequisites

Before you begin, ensure you have the following:

### 1. **DigitalOcean Kubernetes Cluster**
   - A **DigitalOcean** account with an existing Kubernetes cluster.
   - If you haven't set up a Kubernetes cluster on DigitalOcean yet, follow [this guide](https://www.digitalocean.com/docs/kubernetes/quickstart/) to create one.

### 2. **kubectl**
   - Install and configure **kubectl** on your machine to interact with the Kubernetes cluster.
   - Ensure that `kubectl` is properly configured to connect to your DigitalOcean Kubernetes cluster. Use the following command to verify:
   
     ```bash
     kubectl config get-contexts
     ```

### 3. **Docker**
   - **Docker** must be installed on your local machine to build and push container images. Follow [Docker Installation](https://docs.docker.com/get-docker/) for setup.

### 4. **Docker Hub, AWS ECR, Digital Ocean registry or other Private Container Registry**
   - A **Docker Hub** is a free to play platform to host your Docker images.
   - Digital Ocean Container Registry is free to 500Mb(only one repository allowed)

   Links for registration:
   - https://www.docker.com/products/docker-hub/
   - https://www.digitalocean.com/products/container-registry
   - https://aws.amazon.com/ecr/
   

### 5. **doctl** 
  - Install and configure **doctl** on your machine to interact with the Digital Ocean cloud.
  - It helps to automate cluster creation and simplify your interaction with the cloud in general
  - Installation on all platforms described here - https://github.com/digitalocean/doctl/releases
---

## Application Structure

This repository consists of the following components:

### 1. **Web Application**
   - The web application is a **Flask** app that that enable users to register log in and create various notes files.
   - The "backend" part is a postgres database that contains all transaction login and structure for our data 
   - The application is containerized using Docker.

### 2. **Kubernetes Configuration**
   - The **Kubernetes** configuration files are located in the `k8s/` folder:
     - **`flask-deployment.yaml`**: Defines the **Deployment** resource that manages the web app pod.
     - **`flask-service.yaml`**: Defines the internal Kubernetes service to route traffic to the app.
     - **`postgres-deployment.yaml`**: Defines the **Deployment** resource that manages thepostgres database pod.
     - **`postgres-service.yaml`**: Defines the internal Kubernetes service to route traffic to the postgres database.
     - **`loadbalancer.yaml`**: Configures the **LoadBalancer** to expose the app to external traffic.

---

## Kubernetes Cluster Setup

Follow these steps to configure and deploy your application on **DigitalOcean Kubernetes**:

### 1. **Set Up DigitalOcean Kubernetes Cluster**

If you haven't created a Kubernetes cluster yet on DigitalOcean, follow the steps below:

- **Create a Kubernetes Cluster on DigitalOcean**: 
  Go to the [DigitalOcean Kubernetes Setup](https://www.digitalocean.com/docs/kubernetes/quickstart/) page and follow the instructions to create a cluster.
  
- **Access the Cluster**:
  After the cluster is set up, you'll need to configure `kubectl` to access your cluster. Follow these instructions to download the kubeconfig file and set it up:

  ```bash
  doctl kubernetes cluster kubeconfig save <your-cluster-name>
  ```
