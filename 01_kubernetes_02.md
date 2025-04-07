Sure! Here’s the content formatted in Markdown:

```markdown
# Kubernetes Overview

## Introduction
Hello everyone! Can you hear me and see my screen? Please confirm. Alright, let's begin.

## Recap of Previous Discussion
Yesterday, we discussed the Kubernetes architecture and its objects, including:
- **Pods**
- **Replica Sets**
- **Kubernetes Architecture**

### Kubernetes Architecture
The architecture consists of two main parts:
1. **Control Plane**
   - **kube-apiserver**: Acts as the API gateway for communication.
   - **Controller Manager**: Ensures the desired state of the system matches the current state.
   - **Scheduler**: Decides where to run the pods.
   - **etcd**: A key-value store for all Kubernetes data.

2. **Data Plane (Worker Node)**
   - **Container Runtime**: Manages containers (e.g., Docker).
   - **kube-proxy**: Manages networking and traffic.

## Creating an EKS Cluster
We created an EKS cluster by:
1. Launching an EC2 instance.
2. Installing necessary tools (AWS CLI, eksctl, and kubectl).
3. Creating the cluster with the command:
   ```bash
   eksctl create cluster --name <cluster-name> --region <region> --node-type <instance-type>
   ```
4. Confirming the cluster setup with:
   ```bash
   kubectl get nodes
   ```

## Creating Pods
Creating pods directly has a limitation: if a pod is deleted, it won’t be recreated automatically. Instead, we used a **Replica Set**, which ensures that the desired number of pods is always maintained.

### Application Deployment
When upgrading an application, we use a **rolling update** to gradually replace old pods with new ones without downtime. This feature is provided by **Deployments**, not Replica Sets.

### Rolling Updates Process
1. Start with the old version of the application running in several pods.
2. Gradually deploy new versions of the pods.
3. Delete the old version pods one by one.
4. At the end, all pods run the new version with no downtime.

### Example YAML for NGINX Deployment
Here’s an example YAML file for deploying an NGINX application:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 4
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
```
We used the command:
```bash
kubectl apply -f nginx-deployment.yaml
```
To confirm it was running, we used:
```bash
kubectl get pods
```

## Key Takeaways
- **Deployments** manage rolling updates.
- **Replica Sets** ensure a consistent number of pods.
- **Pods** are the actual running containers.

## Managing Kubernetes Objects with kubectl
To manage and inspect Kubernetes objects, you can use:
- `kubectl get deployment` to list all deployments.
- `kubectl describe deployment <deployment-name>` for detailed information.

### Managing Replica Sets
- Use `kubectl get replica set` to list them.
- Use `kubectl describe replica set <replica-set-name>` for details.

### Rolling Updates in Deployments
To create a deployment with a specific version:
1. Create a deployment with the specific version.
2. Apply it using:
   ```bash
   kubectl apply -f <deployment-file.yaml>
   ```
3. Check the status using:
   ```bash
   kubectl get pod
   kubectl get deployment
   ```

## Learning Resources
The best place to learn and get all Kubernetes documentation is the official site [Kubernetes.io](https://kubernetes.io). 

## Conclusion
Remember, don’t rush to finish learning Kubernetes. The key is to go slow, practice consistently, and understand the concepts thoroughly. It takes time, but with patience and regular practice, you'll get there.

---

## Creating and Scaling a Deployment
### Steps to Create a New Deployment
1. Clear existing files if necessary.
2. Create a new file and define the deployment configurations:
   ```yaml
   replicas: 3
   ```
3. Apply the file:
   ```bash
   kubectl apply -f [file_name]
   ```
4. Check the status:
   ```bash
   kubectl get pods
   ```

### Scaling the Deployment
To scale the deployment:
- Use:
  ```bash
  kubectl scale deployment [deployment_name] --replicas=5
  ```
- Alternatively, edit the deployment directly:
  ```bash
  kubectl edit deployment [deployment_name]
  ```

### Safe Scaling Methods
The safest way to modify a deployment is by editing the YAML file and reapplying it:
```bash
kubectl apply -f [file_name]
```

### Updating the Deployment Image
To upgrade or change the version of an application:
1. Edit the YAML file and reapply it.
2. Use `kubectl edit` to update the image version directly.
3. Use:
   ```bash
   kubectl set image deployment/[deployment_name] [container_name]=[new_image]
   ```

## Conclusion
The safest method for modifying Kubernetes deployments is editing the YAML file and reapplying it. The `kubectl edit` method is useful for live changes but can be risky.

---

## Rolling Updates in Kubernetes
### Key Concepts
- **Rolling Update**: Updates your application without downtime by gradually replacing old pods with new ones.
- **Max Unavailable and Max Surge**:
  - **Max Surge**: Defines how many new pods can be created during the update (e.g., 25%).
  - **Max Unavailable**: Defines how many old pods can be unavailable during the update (e.g., 25%).

### Example of a Rolling Update
If you have 100 pods and set Max Surge = 25% and Max Unavailable = 25%:
1. Kubernetes creates 25 new pods with the new image.
2. Once those 25 new pods are running, it deletes 25 old pods.
3. This process continues until all the pods are updated.

### Important Observations
- When a pod is deleted, Kubernetes creates a new one.
- Pod names and IP addresses are ephemeral.

## Networking in Kubernetes
### Internal and External Communication
- **Internal Communication**: Handled by services like ClusterIP.
- **External Communication**: Handled by services like NodePort.

### Key Service Types
- **ClusterIP**: Used for internal communication within the Kubernetes cluster.
- **NodePort**: Used for external communication from outside the cluster.

## Recap
1. Rolling Update allows smooth updates without downtime.
2. Services handle communication between pods (internal or external).
3. Pods are ephemeral, so their IP addresses can change.

---

## Creating a Deployment and ClusterIP Service
### Steps
1. **Create the Deployment**: Responsible for creating and managing Pods.
2. **Create the ClusterIP Service**: Routes traffic to the Pods created by the deployment.
3. **Labels and Selectors**: Used to identify and route traffic to the correct Pods.

### How Traffic Is Routed
- Traffic first hits the service, which then routes it to one of the Pods that match the label selector.

### Service Endpoints
- The Endpoint object keeps track of the IP addresses of the Pods that match the service's label selector.

## Conclusion
The service abstracts communication with the Pods by using the Endpoint to track their IP addresses. This ensures that traffic will always reach the right Pods, even if they change.

---

## Next Steps
- Learn more about NodePort and how it differs from ClusterIP.
- Explore more Kubernetes objects like StatefulSets, ConfigMaps, Secrets, and others.
- Cover real-world application deployment and EFK stack (Elasticsearch, Fluentd, and Kibana) for monitoring.
```
