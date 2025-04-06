	Kubernetes 

Container orchestration is the process of managing and automating the deployment, scaling, and operation of containers. Containers are lightweight and portable, but when you have many containers running across different machines, it can get complex. Container orchestration tools like Kubernetes help you manage all of this by automating tasks like:
	· Deploying containers across multiple machines
	· Scaling containers up or down as needed
	· Managing container networking and communication (Managing means to ensure everything should be correct and work properly)
	· Handling failures and ensuring containers stay up and running

Basically, it's like having a smart system that makes sure all your containers work together smoothly and efficiently.
A container orchestration engine (or tool) is a system that automates the management of containers. It helps with tasks like starting, stopping, and scaling containers, as well as ensuring they run smoothly throughout their lifecycle. In simple terms, it's a tool that makes it easier to handle containers automatically.
The lifecycle of a container refers to everything that happens to it from start to finish. This includes:
	· Deployment: Running the container.
	· Scaling: Adjusting the number of containers, like increasing or decreasing their size or number.
	· Networking: Connecting containers so they can communicate, like one microservice talking to another.
	· Management: Handling all the tasks related to containers, such as monitoring, updates, and maintenance.
Some key features of container orchestration tools include:
	· Automated deployment
	· Scaling containers
	· Service discovery (finding containers)
	· Networking
	· Load balancing
	· Self-healing (fixing issues automatically)
These tools make it easier to manage containers and ensure they work smoothly. 
There are several container orchestration tools, and Kubernetes (often called K8S) is one of the most popular. We call it K8S because there are eight letters between "K" and "S" in the word Kubernetes.
But no, Kubernetes is not the only container orchestration tool. Others include Docker Swarm, Apache Mesos, HashiCorp Nomad, and AWS ECS. While Kubernetes is widely used and holds a large share of the market, learning it in detail will help you understand other orchestration tools as well.  
First, focus on learning Kubernetes in detail. There are other container orchestration tools you can explore later, but understanding Kubernetes well will help you compare it with others. Trying to learn all the tools at once won't be as useful.
Kubernetes is an open-source container orchestration tool created by Google and now managed by CNCF (Cloud Native Computing Foundation). It automates the deployment, scaling, and management of containers. Google originally developed it for internal use, but later made it open-source. CNCF now oversees its development. If you're working with DevOps, you should be familiar with CNCF, as it manages cloud-native tools. 
CNCF (Cloud Native Computing Foundation) is an open-source organization that develops various cloud-native projects, including Kubernetes. They organize events like KubeCon, where people from different companies present their work. Right now, KubeCon is happening in Delhi, and if you're nearby, you can attend. CNCF manages the development and new releases of Kubernetes and many other popular tools used in cloud computing and DevOps. 
So, why do we need Kubernetes? The answer is simple. With the rise of containers and microservices, modern applications need automation, high availability, and scalability. Take the example of Hotstar: during regular streaming, they might only need a few containers. But during events like IPL matches, the number of containers could jump from 2 to 2000, as more users start streaming.
Managing thousands of containers manually would be impossible. That's where Kubernetes comes in. It helps automate scaling, ensures high availability, and optimizes resource usage. For large applications like Netflix, Amazon, or even smaller ones like booking websites, managing containers without an orchestration tool like Kubernetes would be very difficult. That's why we need a container orchestration tool.
Kubernetes solves several important problems. The first one is manual container management. Deploying and managing multiple containers manually across an environment can be complex and time-consuming, but Kubernetes automates this process.
Another issue Kubernetes solves is resource efficiency. If you're launching containers at a large scale without Kubernetes, you may end up with inefficient resource usage, leading to higher costs. Kubernetes helps optimize resource usage and keeps costs in check.
Kubernetes also handles service discovery. For example, if you have a pizza delivery app with a login service and a menu service, Kubernetes helps the login service find where the menu service is running. This is called service discovery — it ensures that different services in your app can find and communicate with each other, even as they scale across different nodes.
We'll dive deeper into concepts like load balancing and networking when we discuss Kubernetes networking in detail. 
When traffic hits your application, Kubernetes decides which container (like the login container) should handle it. This process is called load balancing, and Kubernetes manages it for you. We'll go over how it works in detail later, but just know that Kubernetes handles both service discovery and load balancing to ensure your app runs smoothly. 
Scaling is a very important concept. We'll learn how to scale an application up or down with a simple command, and we can even automate the scaling process. 
Portability across environments is also important. With Kubernetes, you can easily move applications between different environments or even across multiple Kubernetes clusters. Doing this manually would be complex, but Kubernetes makes it simple.
These are the main problems that Kubernetes solves, and it's important to remember them. Sometimes, people don’t understand why they’re learning Kubernetes or why we’re moving from containers to Kubernetes. It's crucial to be clear on why we need such a tool. Without understanding this, learning any tool just becomes about knowing the commands and concepts, but if you're aiming for roles like a solution architect, consultant, or team lead, you need to understand the bigger picture. Once you do, your understanding will be more mature.
Now, let's dive into the Kubernetes architecture.
 
			Kubernetes architecture
We’ve set the context, now let's dive into understanding the Kubernetes architecture.
Kubernetes is a complex topic, so at first, some of you might understand 20%, 30%, or 50% of it, and that's perfectly fine. Kubernetes is like a "cloud within a cloud" because it covers everything you've learned in IT—networking, storage, operating systems, coding, and more. It can be a bit overwhelming, but don't worry. We'll keep revisiting and connecting the dots as we go forward. By the end of the session, everything will make sense, but I'll make sure to go back and clarify concepts as needed. Please trust me, and if you have any questions, let me know. Sound good? 
Kubernetes architecture has two main components: the control plane (also called the master) and the data plane (also called the worker or worker node). In Kubernetes, both terms are commonly used.
The control plane has several components, and one of the key components is the Kube API server. This is part of the control plane.
The control plane has several components. One is the ETCD database, another is the scheduler, and the third is the controller manager. While there are other components, these are the main ones. Let's focus on these first. 
In the worker node, there are a few components. The first is the kubelet, another is the kube proxy, and the third is the container runtime (like Docker or containerd).
These are the seven important components of Kubernetes—four on the master side and three on the worker side. Now, let's understand how they are connected and the role of each one, and we'll dive into more detail soon.  
The Kube API server acts like an API gateway. It's the main way to interact with Kubernetes. For example, if I want to connect to my Kubernetes cluster from my laptop, my request goes to the Kube API server. All components of Kubernetes communicate through the Kube API server—no component talks directly to another. So, even the scheduler and controller manager communicate through the API server. Think of it like an API gateway that handles all communication, both from users and between components. 
The second component is ETCD, which is a key-value database. It stores data in key-value pairs, like a simple database. You might wonder why Kubernetes needs a database—I'll explain that soon. ETCD is an open-source project and is highly reliable, functioning as a distributed database system. Kubernetes uses ETCD internally to store its data. While we won’t dive deep into ETCD itself, it’s important to know that Kubernetes relies on it, and it’s also managed by CNCF. 
The second component is the Scheduler, and it plays a very important role. Let’s say you have two machines: Machine A with 2 GB RAM and 2 CPUs available, and Machine B with 8 GB RAM and 8 CPUs available. If you want to launch an application, the scheduler will choose Machine B because it has more resources.
In Kubernetes, your application runs in containers, and the scheduler decides which worker node (machine) should run the application. If you have multiple worker nodes, the scheduler will run algorithms to decide the best node to run your application, considering available resources like memory, CPU, and overall system performance.
Once the scheduler chooses the best node, it sends this decision to the Kube API server. The Kube API server then communicates with the Kubelet on that node to launch the application. The Kubelet is an agent that runs on each worker node and is responsible for managing the containers. It checks the status of the node and communicates with the Kube API server to keep everything updated.
Then, the Container Runtime (like Docker) is used to run the container. If the container image is not present on the node, the Kubelet will pull it from a container registry like Docker Hub and start the container.
In summary, the scheduler decides where to launch the application, the Kube API server connects to the Kubelet, and the Kubelet manages the container runtime to launch the application.
In simple terms, the Controller Manager in Kubernetes ensures that the system maintains the desired state of the application. Let's break it down:
	• Desired state: This is what you want to happen. For example, if you want 3 login pods (containers) running in your application, that's your desired state.
	• Current state: This is the actual situation. For instance, if only 2 login pods are running instead of the 3 you want, that's the current state.
Now, let’s understand the scenario with a real example:

Example:
	• You want 3 login pods running (desired state).
	• But due to some issue, only 2 login pods are running (current state).
Here’s where the Controller Manager comes into play:
	1. The Controller Manager detects that the current state is not equal to the desired state (you need 3 login pods, but only have 2).
	2. It will inform the Kube API Server to fix this discrepancy.
	3. The API Server checks the actual state by looking into the etcd database (this stores all information about the system’s state).
	4. The API Server confirms that there are 2 login pods running.
	5. It then asks the Scheduler to start a new pod to reach the desired state (3 login pods).
	6. The Scheduler decides on a suitable node to run the new pod.
	7. Finally, the Kubelet (a worker node agent) launches the pod on the chosen node.

Key Points:
	• Controller Manager monitors the desired state versus the current state.
	• If they don't match, it takes action to correct the difference.
	• This is how Kubernetes maintains the desired state of the application automatically.

Other Kubernetes Components:
	• etcd: A database that keeps track of the state of everything in Kubernetes.
	• Container Runtime: Software (like Docker) that runs containers.
	• Kube Proxy: Manages networking and ensures communication between pods.
The system works autonomously to ensure your application is always running as expected, even if things fail or change.


Let's break down what a Pod is in Kubernetes.
In Kubernetes, we don’t deal with containers directly. Instead, we use an abstraction layer called Pod. A Pod is the smallest unit that you deploy in Kubernetes. It can contain one or more containers, which run together on the same node.
To understand this, let’s think of a Pod as a group of containers that share certain resources, such as networking and storage. For example, a Pod might contain a container for an application and another container for its supporting services. These containers inside a Pod can communicate with each other easily because they share the same IP address and port space.
Now, why do we use Pods instead of just containers? Containers by themselves are isolated and don’t share resources with others, which can make some tasks more complex. Pods allow related containers to run together, making it easier to manage them.
For example, imagine you have an application (e.g., a login service) running in a container. Instead of just running the container alone, Kubernetes organizes it into a Pod, which can contain additional containers for supporting tasks like logging or monitoring.
In short:
	· A Pod is the smallest deployable unit in Kubernetes.
	· It can contain one or more containers that share the same resources.
	· Containers in a Pod can easily communicate with each other because they share the same networking and storage.
We use Pods to manage related containers together in a more efficient and organized way.
 
Let's simplify the explanation:
Imagine you're the head of Domino's in India, and you want to know which region (Delhi, Bangalore, etc.) is ordering more pizzas. Normally, you could collect the IP address of the users from the login page to determine their location and analyze the data. Technically, yes, you could add that logic directly to the login application.
But here's the catch: microservices are meant to handle one specific task. If you add this logic for tracking user IPs directly in the login service, you would be making the application more complex and going back to a monolithic approach, which defeats the purpose of microservices. Microservices should only focus on one thing.
Instead, Kubernetes offers a solution: Pods. A Pod can contain multiple containers. For instance, your main container handles the login logic, while another container inside the same Pod can collect the IP addresses and process the data separately. This way, your main container stays focused on the core business logic, and the additional logic is offloaded to a separate container, keeping things simple and modular.
Most of the time, Pods will only have one container, but sometimes you'll need more than one container inside a Pod to handle different tasks that are related but separate.
So, in Kubernetes:
	· Pods are the smallest deployable units.
	· They can contain one or more containers that share the same network and storage resources.
	· You don’t directly create containers in Kubernetes; they are automatically created inside a Pod.
This separation helps maintain efficiency and clarity in the application design, avoiding unnecessary complexity in a single container.
 
Let’s break it down in simpler terms:
Launching a Kubernetes Cluster
There are several ways to launch a Kubernetes cluster. Some common services to do this are:
	· EKS (Elastic Kubernetes Service) from AWS
	· AKS (Azure Kubernetes Service) from Microsoft Azure
	· GKE (Google Kubernetes Engine) from Google Cloud
	· OKE (Oracle Kubernetes Engine) from Oracle
	· IKS (IBM Kubernetes Service) from IBM
These services are called managed Kubernetes services. The key difference between a managed service and a regular Kubernetes setup is that in managed services, the cloud provider manages the control plane (master node) for you.
Why Managed Kubernetes Services?
In these services, the cloud provider handles the master node for you, so you don’t have to worry about managing it. You only manage the worker nodes where your actual applications are running. The benefit here is that you don’t need to maintain high availability, security, or patching for the control plane; it’s handled by the cloud provider.
Other Ways to Launch Kubernetes
	1. EC2 Setup (Manual Kubernetes Cluster):
			o You can manually create an EC2 instance (virtual machine) as a master and others as worker nodes.
			o You have to manually set up the Kubernetes components and handle the management of the cluster, including high availability and backups.
	2. MiniKube (For Testing or Learning):
			o MiniKube is a lightweight Kubernetes setup, often used for learning or testing purposes.
			o It runs a single-node Kubernetes cluster on your local machine, where the control and worker node are the same. This setup is easy to manage but not suitable for production.
EKS and Kubernetes Command Tools
	· EKS simplifies Kubernetes management by handling the control plane for you.
	· To interact with EKS, you'll often use tools like: 
			o AWS CLI (Command Line Interface)
			o EKS-CTL (EKS Control) for launching and managing EKS clusters
			o kubectl (or sometimes called kubekettle) to interact with Kubernetes clusters

In EKS, you don’t have to deal with the complexities of managing control plane nodes or their availability. You can focus on deploying and running your applications.

Setting up EKS
To launch an EKS cluster, you’ll need:
	1. EC2 instance: You'll create an EC2 instance (virtual machine).
	2. AWS CLI: Install AWS’s command-line tool to manage AWS services.
	3. EKS-CTL: This is a simple tool that makes it easier to create and manage EKS clusters.
	4. kubectl: This is the tool you use to interact with Kubernetes.
With these tools installed, you can easily launch your EKS cluster and start deploying applications.

In summary:
	· Managed Kubernetes services (like EKS, AKS, GKE) make it easier for you to run Kubernetes without worrying about the control plane.
	· MiniKube is a good option for learning or testing.
	· EKS is one of the easiest ways to manage Kubernetes clusters on AWS.
 
What is kubectl?
kubectl (or sometimes called kubekettle) is a command-line tool that allows you to interact with your Kubernetes cluster. Think of it like a browser for Kubernetes, just like you need a browser (like Chrome or Firefox) to connect to websites. In this case, kubectl is the tool you use to connect to your Kubernetes API server and manage your cluster.
When you run commands using kubectl, it sends requests to the Kubernetes API server to perform actions on your cluster, such as deploying applications or managing resources.
Steps to Set Up EKS Cluster
	1. Install Required Tools:
			o AWS CLI: A tool for managing AWS services from the command line.
			o EKS-CTL: A utility for managing EKS clusters.
			o kubectl: The command-line tool for interacting with Kubernetes clusters.
	2. Assign IAM Role to EC2 Instance:
			o You need to assign a role to your EC2 instance. This role should have permissions such as:
					§ EC2 full access
					§ VPC full access
					§ S3 full access
			o For testing purposes, you can use an admin policy, but in production, you would create a more restrictive role with the necessary permissions.
	3. Launch an EC2 Instance:
			o You’ll create an EC2 instance (a virtual machine in AWS). This instance will be used to interact with your EKS cluster, not as a worker node.
			o It's recommended to use at least a T2 Small instance or higher for better performance.
	4. Install Necessary Tools on EC2:
			o Once the EC2 instance is running, you'll need to ensure that the AWS CLI, EKS-CTL, and kubectl are installed on the instance.
	5. Connect to EC2:
			o After setting up the EC2 instance, you can connect to it using SSH to run the necessary commands to configure and interact with your EKS cluster.
Summary
	· kubectl helps you manage Kubernetes clusters via the command line.
	· To set up an EKS cluster, you'll need an EC2 instance with tools like AWS CLI, EKS-CTL, and kubectl installed.
	· The EC2 instance needs a role with permissions to interact with AWS services like EC2, VPC, and S3.
This way, you'll be able to manage your EKS cluster and deploy applications on it.
 
Simplified Overview:
	1. Assigning a Role to EC2 Instance:
			o Go to your EC2 instance, click "Actions," then "Security," and select "Modify IAM Role."
			o Choose the AWS Admin role (or create a custom role with permissions like EC2, S3, and VPC full access) and apply it.
	2. Installing AWS CLI, kubectl, and EKS-CTL:
			o AWS CLI: This is already done.
			o kubectl: 
					§ Use curl to download kubectl.
					§ Make it executable with chmod +x kubectl.
					§ Move it to /usr/local/bin to add it to your PATH.
					§ Verify installation using kubectl version.
			o EKS-CTL: 
					§ Download and extract the tool using a similar process.
					§ Move the extracted file to /usr/local/bin.
	3. Creating an EKS Cluster:
			o Use the command: 
			o eksctl create cluster --name my-cluster --region <region> --node-type t2.medium --nodes 2
This will create a Kubernetes cluster with 2 nodes in your specified region (like Mumbai).
	4. Cluster Creation Process:
			o The cluster will take around 10-15 minutes to be created.
			o You can track progress in AWS CloudFormation or the EC2 console to see the worker nodes being launched.
	5. Connecting to the Cluster:
			o The control plane (API server) is managed by AWS.
			o Worker nodes run the actual applications, and you interact with the cluster using kubectl commands from your EC2 instance (client machine).
	6. Kubernetes Object Deployment:
			o Kubernetes uses two approaches for deploying objects: Declarative and Imperative. 
					§ Declarative: You create a YAML file defining the object (e.g., pod, deployment) and apply it using kubectl apply -f <file>.
					§ Imperative: You directly use kubectl commands to create and manage objects.
	7. Cluster Architecture:
			o Master Node: Runs the control plane (API server) and manages the cluster.
			o Worker Nodes: Run applications and services.
			o Your EC2 Instance: Acts as the client machine from where you issue kubectl commands to interact with the cluster.
Conclusion:
In summary, you're creating an EKS cluster with two worker nodes, installing necessary tools (kubectl, eksctl), and managing the cluster from a client machine (EC2 instance). The control plane is managed by AWS, and worker nodes run your applications. You'll typically use the declarative approach for deploying resources, using YAML files and kubectl commands.
 
Let's simplify the explanation:
	1. Creating a Pod:
			o First, you need to create a YAML file for the pod (we'll call it my-pod.yaml).
			o Use the following command to apply this YAML file: 
			o kubectl apply -f my-pod.yaml
			o This command will create a pod based on the specifications inside the YAML file.
	2. Pod Status:
			o To check the status of the pod, use: 
			o kubectl get pods
			o The pod will be in a Running state once it's successfully created.
	3. Understanding the YAML File:
			o The YAML file specifies the details of the pod, like the image to be used (e.g., nginx).
	4. Behind the Scenes:
			o When you run the kubectl apply -f my-pod.yaml command: 
					§ It sends the instructions to the Kubernetes API Server.
					§ The Scheduler determines which node (worker machine) is best suited for the pod based on resources.
					§ The scheduler then tells the appropriate node to run the pod.
					§ The node's container runtime (like runC) will download the specified image (e.g., nginx) from a repository (like Docker Hub) if it's not already available and run the pod.
Key Concepts:
	· kubectl apply: Command to apply configurations to the cluster.
	· Scheduler: Decides which node should run the pod.
	· Container Runtime (runC): Pulls the container image and runs the pod.
That's the process in simple terms!
 
Let's break it down into simpler points:
	1. Creating a Pod:
			o When you create a pod, you specify a container image (like nginx).
			o The pod gets created, and Kubernetes uses various components (API server, scheduler, and container runtime) to manage the pod.
	2. Deleting a Pod:
			o If you delete a pod manually using the command kubectl delete pod <pod-name>, the pod is removed.
			o However, pods don't recreate themselves unless they're part of a higher-level object like a ReplicaSet.
	3. ReplicaSet:
			o A ReplicaSet ensures that a specific number of pods (e.g., 3) are always running.
			o If a pod is deleted, the ReplicaSet will automatically recreate a new one to match the desired number of pods.
			o You can define the number of pods you want using the replicas field in the ReplicaSet configuration file (e.g., replicas: 3).
	4. How the ReplicaSet Works:
			o When you delete a pod, Kubernetes notices that the desired state (3 pods) doesn't match the current state (2 pods).
			o The Controller Manager takes action, tells the Scheduler to find a node, and then the system recreates the missing pod on the node.
			o This process happens very quickly.
	5. Automatic Pod Replacement:
			o If you delete a pod, the ReplicaSet will automatically create a new one. This ensures that your desired number of pods (e.g., 3) is always maintained.
	6. Deleting All Pods:
			o If you delete all the pods managed by the ReplicaSet, Kubernetes will recreate them all again, maintaining the desired count.
	7. Not Autoscaling:
			o This process is not autoscaling. Autoscaling automatically adjusts the number of pods based on resource usage or other metrics. The ReplicaSet only ensures the desired number of pods is maintained, not scaling based on demand.
	8. Conclusion:
			o A ReplicaSet is a better option than directly creating pods because it ensures the desired state is maintained automatically.
This is how Kubernetes maintains and manages pod replicas automatically, making sure your application always has the right number of pods running.
 
Let’s break this down:
	1. The Process of Creating Pods:
			o The process starts with writing application code.
			o From this code, a Dockerfile is created.
			o The Dockerfile is used to build a Docker image.
			o Then, this image is used to create a pod. Multiple pods can be created from the same image (e.g., 3 pods).
	2. Upgrading the Application:
			o To upgrade, you modify your code and create a new Dockerfile.
			o From the new Dockerfile, you build a new Docker image.
			o Then, you need to update your pods with the new image.
			o However, if you delete all the old pods and create new ones, there’s downtime, meaning the application will be unavailable for a short time.
	3. Avoiding Downtime with Rolling Updates:
			o Instead of deleting all pods at once, Kubernetes supports rolling updates to ensure no downtime.
			o Rolling updates allow you to update your pods without taking your application offline.
			o This feature isn’t available with ReplicaSets alone, but it is part of a Deployment object.
			o A Deployment manages rolling updates and ensures the application remains available.
	4. Next Steps:
			o Tomorrow, we'll dive into Deployments, how to write them, and how to handle application updates without downtime.
			o We'll also look at how to scale applications and other important features of deployments.
	5. Answering Questions:
			o If you delete a pod, the container inside it is also deleted because the pod is the smallest unit that runs the container.
			o We'll cover rolling updates tomorrow to show how to update containers without downtime.
	6. Kubernetes Overview:
			o Kubernetes may feel complex at first, but it's an essential tool for managing containers.
			o It’s highly in-demand and will become more enjoyable as you understand it.
We’ll revisit these concepts tomorrow and clear up any confusion. Thank you for your time today!

