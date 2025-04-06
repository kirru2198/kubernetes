# Kubernetes Overview

## What is Container Orchestration?

It is a system for automating the lifecycle of a container.

Container orchestration is the process of managing and automating the deployment, scaling, and operation of containers. Containers are lightweight and portable, but when you have many containers running across different machines, it can get complex. Container orchestration tools like Kubernetes help you manage all of this by automating tasks like:

- Deploying containers across multiple machines
- Scaling containers up or down as needed
- Managing container networking and communication (Managing means to ensure everything should be correct and work properly or rightly- right thing to right thing type)
- Handling failures and ensuring containers stay up and running

Acronym - Drive Success Maintain Harmony

Basically, it's like having a smart system that makes sure all your containers work together smoothly and efficiently.

## What is Kubernetes?

A container orchestration engine (or tool) is a system that automates the management of containers. It helps with tasks like starting, stopping, and scaling containers, as well as ensuring they run smoothly throughout their lifecycle. In simple terms, it's a tool that makes it easier to handle containers automatically.

### Container Lifecycle

The lifecycle of a container refers to everything that happens to it from start to finish. This includes:

- **Deployment**: Running the container.
- **Scaling**: Adjusting the number of containers.
- **Networking**: Connecting containers for communication. (networking means one micro service wants to connect with another microservice that what networking of the container ----> service discovery)
- **Management**: Handling tasks like monitoring, updates, and maintenance. (Management = the word management include everything -- all the administration part of the container -----> load balancing, self-healing)

### Key Features of Container Orchestration Tools

- Automated deployment
- Scaling containers
- Service discovery
- Networking
- Load balancing
- Self-healing

These tools make it easier to manage containers and ensure they work smoothly.

## Why Kubernetes?

Kubernetes (often called K8S) is one of the most popular container orchestration tools. It was created by Google and is now managed by the Cloud Native Computing Foundation (CNCF). With the rise of containers and microservices, modern applications need automation, high availability, and scalability. For example, during events like IPL matches, the number of containers for a streaming service might jump from 2 to 2000.

### Problems Solved by Kubernetes

1. **Manual Container Management**: Automates the deployment and management of multiple containers.
2. **Resource Efficiency**: Optimizes resource usage to keep costs in check.
3. **Service Discovery**: Ensures different services in your app can find and communicate with each other.
4. **Load Balancing**: Manages traffic to ensure smooth application performance.
5. **Scaling**: Allows for easy scaling of applications up or down.
6. **Portability Across Environments**: we can have multiple environments within Kubernetes cluster or we can have multiple Kubernetes clusters --- Portability across environments is also important. With Kubernetes, you can easily move applications between different environments or even across multiple Kubernetes clusters. Doing this manually would be complex, but Kubernetes makes it simple.

## Kubernetes Architecture

Kubernetes architecture has two main components: the **control plane** (master) and the **data plane** (worker nodes).

<img width="607" alt="image" src="https://github.com/user-attachments/assets/371a9b34-ff03-4178-8bbc-a840e3a1a2b8" />

### Control Plane Components

1. **Kube API Server**: The main way to interact with Kubernetes.
2. **ETCD**: A key-value database that stores data in key-value pairs.
3. **Scheduler**: Decides which worker node should run the application.
4. **Controller Manager**: Ensures the system maintains the desired state of the application.

### Worker Node Components

1. **Kubelet**: An agent that runs on each worker node and manages containers.
2. **Kube Proxy**: Manages networking and ensures communication between pods.
3. **Container Runtime**: Software (like Docker) that runs containers.

## Understanding Pods

In Kubernetes, we don’t deal with containers directly. Instead, we use an abstraction layer called **Pod**. A Pod is the smallest unit that you deploy in Kubernetes. It can contain one or more containers that run together on the same node.

### Key Points about Pods

- A Pod is the smallest deployable unit in Kubernetes.
- It can contain one or more containers that share the same resources.
- Containers in a Pod can easily communicate with each other.

## Launching a Kubernetes Cluster

There are several ways to launch a Kubernetes cluster, including:

- **Managed Kubernetes Services**: 
  - EKS (Elastic Kubernetes Service from AWS)
  - AKS (Azure Kubernetes Service from Microsoft Azure)
  - GKE (Google Kubernetes Engine from Google Cloud)
  - OKE (Oracle Kubernetes Engine from Oracle)
  - IKS (IBM Kubernetes Service from IBM)

### Why Managed Kubernetes Services?

In managed services, the cloud provider handles the control plane for you, allowing you to focus on managing the worker nodes where your applications run.

### Other Ways to Launch Kubernetes

1. **EC2 Setup (Manual Kubernetes Cluster)**: Manually create an EC2 instance as a master and others as worker nodes.
2. **MiniKube**: A lightweight Kubernetes setup for learning or testing purposes.

## kubectl

**kubectl** (or sometimes called kubekettle) is a command-line tool that allows you to interact with your Kubernetes cluster. It sends requests to the Kubernetes API server to perform actions on your cluster.

### Steps to Set Up EKS Cluster

1. **Install Required Tools**:
   - AWS CLI
   - EKS-CTL
   - kubectl

2. **Assign IAM Role to EC2 Instance**: Assign a role with necessary permissions.

3. **Launch an EC2 Instance**: Create an EC2 instance to interact with your EKS cluster.

4. **Install Necessary Tools on EC2**: Ensure AWS CLI, EKS-CTL, and kubectl are installed.

5. **Connect to EC2**: Use SSH to connect to the instance.

### Summary

- kubectl helps you manage Kubernetes clusters via the command line.
- To set up an EKS cluster, you'll need an EC2 instance with tools like AWS CLI, EKS-CTL, and kubectl installed.
- The EC2 instance needs a role with permissions to interact with AWS services.

## Creating and Managing Pods

### Creating a Pod

1. Create a YAML file for the pod (e.g., `my-pod.yaml`).
2. Use the command: 
   ```bash
   kubectl apply -f my-pod.yaml
Here's the provided content written in Markdown format:
---
# Checking Pod Status

To check the status of the pod, use:

```bash
kubectl get pods
```

## Understanding the YAML File

The YAML file specifies the details of the pod, like the image to be used (e.g., `nginx`).

## Behind the Scenes

When you run the `kubectl apply -f my-pod.yaml` command:

- It sends instructions to the Kubernetes API Server.
- The Scheduler determines which node is best suited for the pod.
- The node's container runtime pulls the specified image and runs the pod.

## ReplicaSets

A ReplicaSet ensures that a specific number of pods are always running. If a pod is deleted, the ReplicaSet will automatically recreate a new one to match the desired number of pods.

### Key Points about ReplicaSets

- If you delete a pod, the ReplicaSet will automatically create a new one.
- This ensures that your desired number of pods is always maintained.
- This process is not autoscaling; it only maintains the desired state.

## Conclusion

Kubernetes may feel complex at first, but it's an essential tool for managing containers. It’s highly in-demand and will become more enjoyable as you understand it. We’ll revisit these concepts and dive deeper into Deployments and scaling applications in future sessions. Thank you for your time today!
```

