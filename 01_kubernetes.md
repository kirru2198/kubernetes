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

<img width="476" alt="image" src="https://github.com/user-attachments/assets/6d1679df-6765-4431-9553-16f57031a021" />

Here, i have multiple microservices code, i want to run it. i wnat to run multiple copies of my code.

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

---
The Kube API server acts like an API gateway. It's the main way to interact with Kubernetes. For example, if I want to connect to my Kubernetes cluster from my laptop, my request goes to the Kube API server. All components of Kubernetes communicate through the Kube API server—no component talks directly to another. So, even the scheduler and controller manager communicate through the API server. Think of it like an API gateway that handles all communication, both from users and between components. 

The second component is ETCD, which is a key-value database. It stores data in key-value pairs, like a simple database. You might wonder why Kubernetes needs a database—I'll explain that soon. ETCD is an open-source project and is highly reliable, functioning as a distributed database system. Kubernetes uses ETCD internally to store its data. While we won’t dive deep into ETCD itself, it’s important to know that Kubernetes relies on it, and it’s also managed by CNCF. 

The second component is the Scheduler, and it plays a very important role. Let’s say you have two machines: Machine A with 2 GB RAM and 2 CPUs available, and Machine B with 8 GB RAM and 8 CPUs available. If you want to launch an application, the scheduler will choose Machine B because it has more resources.

In Kubernetes, your application runs in containers, and the scheduler decides which worker node (machine) should run the application. If you have multiple worker nodes, the scheduler will run algorithms to decide the best node to run your application, considering available resources like memory, CPU, and overall system performance.

Once the scheduler chooses the best node, it sends this decision to the Kube API server. The Kube API server then communicates with the Kubelet on that node to launch the application. The Kubelet is an agent that runs on each worker node and is responsible for managing the containers. It checks the status of the node and communicates with the Kube API server to keep everything updated.

Then, the Container Runtime (like Docker) is used to run the container. If the container image is not present on the node, the Kubelet will pull it from a container registry like Docker Hub and start the container.

In summary, the scheduler decides where to launch the application, the Kube API server connects to the Kubelet, and the Kubelet manages the container runtime to launch the application.

In simple terms, the Controller Manager in Kubernetes ensures that the system maintains the desired state of the application. Let's break it down:
 - Desired state: This is what you want to happen. For example, if you want 3 login pods (containers) running in your application, that's your desired state.
 - Current state: This is the actual situation. For instance, if only 2 login pods are running instead of the 3 you want, that's the current state.

Now, let’s understand the scenario with a real example:

Example:
 - You want 3 login pods running (desired state).
 - But due to some issue, only 2 login pods are running (current state).

Here’s where the Controller Manager comes into play:
1. The Controller Manager detects that the current state is not equal to the desired state (you need 3 login pods, but only have 2).
2. It will inform the Kube API Server to fix this discrepancy.
3. The API Server checks the actual state by looking into the etcd database (this stores all information about the system’s state).
4. The API Server confirms that there are 2 login pods running.
5. It then asks the Scheduler to start a new pod to reach the desired state (3 login pods).
6. The Scheduler decides on a suitable node to run the new pod.
7. Finally, the Kubelet (a worker node agent) launches the pod on the chosen node.

- Kube Proxy: Manages networking and ensures communication between pods.

The system works autonomously to ensure your application is always running as expected, even if things fail or change.

 ---

## Understanding Pods

In Kubernetes, we don’t deal with containers directly. Instead, we use an abstraction layer called **Pod**. A Pod is the smallest unit that you deploy in Kubernetes. It can contain one or more containers that run together on the same node.

### Key Points about Pods

- A Pod is the smallest deployable unit in Kubernetes.
- It can contain one or more containers that share the same resources.
- Containers in a Pod can easily communicate with each other because they share the same networking and storage.

To understand this, let’s think of a Pod as a group of containers that share certain resources, such as networking and storage. For example, a Pod might contain a container for an application and another container for its supporting services. These containers inside a Pod can communicate with each other easily because they share the same IP address and port space.

Now, why do we use Pods instead of just containers? Containers by themselves are isolated and don’t share resources with others, which can make some tasks more complex. Pods allow related containers to run together, making it easier to manage them.

For example, imagine you have an application (e.g., a login service) running in a container. Instead of just running the container alone, Kubernetes organizes it into a Pod, which can contain additional containers for supporting tasks like logging or monitoring.

We use Pods to manage related containers together in a more efficient and organized way.

Example:

Imagine you're the head of Domino's in India, and you want to know which region (Delhi, Bangalore, etc.) is ordering more pizzas. Normally, you could collect the IP address of the users from the login page to determine their location and analyze the data. Technically, yes, you could add that logic directly to the login application.

But here's the catch: microservices are meant to handle one specific task. If you add this logic for tracking user IPs directly in the login service, you would be making the application more complex and going back to a monolithic approach, which defeats the purpose of microservices. Microservices should only focus on one thing. (= Here monolithic means all the code of each microservices in a single code, but microservice approach means one logic and it's dependencies)

<img width="299" alt="image" src="https://github.com/user-attachments/assets/ee028cf0-88ce-4bd6-8c55-475e2619e120" />
<img width="560" alt="image" src="https://github.com/user-attachments/assets/13017718-245e-4f7e-95cf-fb20aca73b97" />

Instead, Kubernetes offers a solution: Pods. A Pod can contain multiple containers. For instance, your main container handles the login logic, while another container inside the same Pod can collect the IP addresses and process the data separately. This way, your main container stays focused on the core business logic, and the additional logic is offloaded to a separate container, keeping things simple and modular.

Most of the time, Pods will only have one container, but sometimes you'll need more than one container inside a Pod to handle different tasks that are related but separate.

## Launching a Kubernetes Cluster

There are several ways to launch a Kubernetes cluster, including:

- **Managed Kubernetes Services**: 
  - EKS (Elastic Kubernetes Service from AWS)
  - AKS (Azure Kubernetes Service from Microsoft Azure)
  - GKE (Google Kubernetes Engine from Google Cloud)
  - OKE (Oracle Kubernetes Engine from Oracle)
  - IKS (IBM Kubernetes Service from IBM)

### Why Managed Kubernetes Services?

In managed services, the cloud provider handles the control plane for you, allowing you to focus on managing the worker nodes where your applications run. The benefit here is that you don’t need to maintain high availability, security, or patching for the control plane; it’s handled by the cloud provider.

<img width="553" alt="image" src="https://github.com/user-attachments/assets/84c252b8-8df0-466c-aefa-acb4e2466b61" />


### Other Ways to Launch Kubernetes

1. **EC2 Setup (Manual Kubernetes Cluster)**: Manually create an EC2 instance as a master and others as worker nodes.
 - You have to manually set up the Kubernetes components and handle the management of the cluster, including high availability and backups.

2. **MiniKube**: A lightweight Kubernetes setup for learning or testing purposes.
 - It runs a single-node Kubernetes cluster on your local machine, where the control and worker node are the same. This setup is easy to manage but not suitable for production.

We can launch EKS cluster using - Terraform, cloudFormation, AWS-CLI and eksctl (tool)
---
In 2016, AWS introduced EKS (Elastic Kubernetes Service). Before that, AWS had ECS (Elastic Container Service), which is similar to Docker Swarm but doesn't use Kubernetes. Around 2014-2016, Kubernetes became very popular, and other cloud providers like Azure and Google started offering managed Kubernetes services. In response, AWS launched EKS in 2016 as a managed Kubernetes service.

At the time, I was working as a Solution Architect at AWS, primarily handling ECS-related issues. When EKS was launched, it was a bit more complex to set up compared to other services like GKE (Google Kubernetes Engine) or AKS (Azure Kubernetes Service), where setting up a cluster was easy with just a few clicks. AWS made the process a bit more complicated, which led to a company developing a tool called **eksctl**. This tool simplified launching an EKS cluster, and eventually, AWS acquired that company.

I've personally never launched an EKS cluster from the AWS console because it seemed too complicated. Instead, I always used **eksctl**, which made the process much simpler. I'll show you how to use **eksctl** to launch an EKS cluster easily, which is the simplest way to set one up.

## kubectl

**kubectl** (or sometimes called kubekettle) is a command-line tool that allows you to interact with your Kubernetes cluster. It sends requests to the Kubernetes API server to perform actions on your cluster.

Think of it like a browser for Kubernetes, just like you need a browser (like Chrome or Firefox) to connect to websites. In this case, kubectl is the tool you use to connect to your Kubernetes API server and manage your cluster.

When you run commands using kubectl, it sends requests to the Kubernetes API server to perform actions on your cluster, such as deploying applications or managing resources.

---
### Steps to Set Up EKS Cluster
---

<img width="290" alt="image" src="https://github.com/user-attachments/assets/44441a96-b820-4bef-aa17-eaa8609b631e" />


1. **Install Required Tools**:
- Create a EC2 instance first
   - AWS CLI (in AWS Linux it's a default feature)
   - EKS-CTL
   - kubectl

3. **Assign IAM Role to EC2 Instance**: Assign a role with necessary permissions.
- You need to assign a role to your EC2 instance. This role should have permissions such as:
   - EC2 full access
   - VPC full access
   - S3 full access
- For testing purposes, you can use an admin policy, but in production, you would create a more restrictive role with the necessary permissions.

Implimentation:
---
1. **Launch an EC2 Instance**: Create an EC2 instance to interact with your EKS cluster.
<img width="68" alt="image" src="https://github.com/user-attachments/assets/bedda9a4-2d6f-4754-8b81-48b6aabb5ef6" />
<img width="608" alt="image" src="https://github.com/user-attachments/assets/014da10c-008e-4f89-ac50-222908b95a22" />
<img width="620" alt="image" src="https://github.com/user-attachments/assets/a602ad24-d0ad-4d67-8418-ca8b75365e78" />



   

2. **Install Necessary Tools on EC2**: Ensure AWS CLI, EKS-CTL, and kubectl are installed.

4. **Connect to EC2**: Use SSH to connect to the instance.

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

