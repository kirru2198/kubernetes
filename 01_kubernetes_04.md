# Kubernetes Concepts for Real-World Applications

## Introduction
Good morning, good evening everyone! I hope my voice and screen are clear. Can you confirm that you can see my screen? 

Today, we'll cover important Kubernetes concepts required for deploying real-world applications. We’ll study the concepts first and then move to the lab.

---

## 1. Namespaces in Kubernetes
A namespace is like a virtual cluster within a physical Kubernetes cluster. In simple terms, it's a way to divide your Kubernetes cluster into smaller, isolated parts. Imagine your Kubernetes cluster as an apartment building. The entire building is the Kubernetes cluster, and each flat in that building is a namespace. Each flat (namespace) can have its own resources and applications, so the resources are isolated between flats (namespaces).

### Why Use Namespaces?
- **Resource Division**: Helps divide the cluster's resources between different teams or users.
- **Resource Isolation**: Resources in one namespace are separate from those in another. For example, Team A can run their application in namespace A, and Team B can run their app in namespace B.

### Default Namespaces
Kubernetes automatically creates some namespaces, such as:
- **default**: Where resources are created if no namespace is specified.
- **kube-system**: Used by Kubernetes for system components (like DNS, kube-proxy).
- **kube-node-lease**: Used for node communication; don’t modify it.
- **kube-public**: For resources that are accessible by everyone.

### How to Use Namespaces
1. To list namespaces, run:
   ```bash
   kubectl get namespaces
   ```
2. To create a namespace, create a YAML file like this:
   ```yaml
   apiVersion: v1
   kind: Namespace
   metadata:
     name: my-namespace
   ```
   Apply it with:
   ```bash
   kubectl apply -f namespace.yaml
   ```
3. To create resources in a specific namespace, specify the namespace in your resource file:
   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: my-deployment
     namespace: my-namespace
   ```

### Example
After creating a namespace, you can check if it’s created with:
```bash
kubectl get namespaces
```

### Summary
Namespaces allow you to divide a Kubernetes cluster into isolated sections, making it easier to manage resources for different teams. When creating resources, always specify the namespace to avoid using the default one.

---

## 2. ConfigMap Concept
Let’s now talk about **ConfigMaps**. A ConfigMap is a Kubernetes resource used to store configuration data for your applications. For example, your app might need environment variables, configuration files, or command-line arguments. Instead of hardcoding them in your application, you can store them in a ConfigMap.

### Why Use ConfigMaps?
- **Decoupling Configuration**: Store configuration separately from your application, allowing easier updates without modifying the app’s code or rebuilding Docker images.
- **Dynamic Updates**: If your configuration changes, you can update the ConfigMap without changing your application.

### Example of a ConfigMap YAML
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  app_environment: production
  debug: "false"
  log_level: info
```
You can create this ConfigMap using:
```bash
kubectl apply -f configmap.yaml
```
After it’s created, you can check it with:
```bash
kubectl get configmaps
kubectl describe configmap app-config
```

### Using ConfigMaps in Applications
Once a ConfigMap is created, you can reference it in your application pods. For example, you might inject the configuration values as environment variables.

### Summary
Namespaces help you organize and isolate resources in Kubernetes, while ConfigMaps let you manage configuration data separately from your application, making it more flexible and easier to update.

---

## 3. Using ConfigMaps in Pods
To use the ConfigMap in your application, you can reference it inside a pod or a deployment. For example, if you want to use it in a pod, you can define an environment variable in the pod's YAML file, like this:
```yaml
env:
  - name: APP_ENVIRONMENT
    valueFrom:
      configMapRef:
        name: app-config
        key: app_environment
  - name: DEBUG
    valueFrom:
      configMapRef:
        name: app-config
        key: debug
```
Once the pod is running, the values from the ConfigMap (like `APP_ENVIRONMENT` and `DEBUG`) will be set as environment variables in the pod. If you want to change the values, just modify the ConfigMap and restart the pod. The updated values will be used automatically.

### Why Use ConfigMap?
- Change environment variables or configuration values without needing to rebuild Docker images.
- A clean and dynamic way to handle configuration in Kubernetes, especially across different environments (e.g., dev, staging, production).

---

## 4. Secrets in Kubernetes
Next, you might use **Secrets** for sensitive data like passwords. Secrets work similarly to ConfigMaps, but the difference is that the data is encrypted.

### Creating a Secret
1. Encrypt sensitive data (like a password) using base64:
   ```bash
   echo -n "my-password" | base64
   ```
2. Define the secret in a YAML file:
   ```yaml
   apiVersion: v1
   kind: Secret
   metadata:
     name: db-secret
   type: Opaque
   data:
     username: <base64-encoded-username>
     password: <base64-encoded-password>
   ```

### Referencing a Secret in a Pod
You can reference the secret in a pod YAML file to make it available as environment variables:
```yaml
env:
  - name: DB_USERNAME
    valueFrom:
      secretKeyRef:
        name: db-secret
        key: username
  - name: DB_PASSWORD
    valueFrom:
      secretKeyRef:
        name: db-secret
        key: password
```

### Key Points
- **Secrets** store sensitive information, and Kubernetes encrypts the values by default (using base64).
- **ConfigMaps** store non-sensitive configuration data (like environment variables), and the values are visible in plain text.
- Use Secrets for storing passwords, API keys, and other sensitive information.
- Use ConfigMaps for regular app configurations and settings.

---

## 5. DaemonSet in Kubernetes
### What is a DaemonSet?
A DaemonSet is used when you need to run a pod on every node in the cluster. For example, if you're collecting logs, you want a pod running on each node to collect logs from all nodes in the cluster.

### Key Points of DaemonSet
- A DaemonSet ensures that a pod is scheduled on all nodes.
- You don't define the number of replicas; the system automatically schedules one pod per node.
- If a node is added or removed, DaemonSet will automatically create or delete pods to match the current nodes.

### Example of DaemonSet YAML
```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: my-daemonset
spec:
  selector:
    matchLabels:
      name: my-daemon
  template:
    metadata:
      labels:
        name: my-daemon
    spec:
      containers:
      - name: my-container
        image: nginx
```

### Why Use a DaemonSet?
If you need an application (like a log collector) running on every node, you use a DaemonSet. A Deployment won't guarantee that your pods will be evenly distributed across nodes, but a DaemonSet will ensure that your application runs on every node.

---

## 6. StatefulSet in Kubernetes
### Stateless vs. Stateful Applications
- **Stateless Applications**: Do not rely on their previous state (e.g., a web application where each request is independent).
- **Stateful Applications**: Rely on data or state between sessions (e.g., a database needs to keep track of its state over time).

### What is a StatefulSet?
A StatefulSet in Kubernetes is used to manage stateful applications. It ensures that each pod gets a unique identity and stable network identity (e.g., a persistent storage volume). StatefulSets are ideal for applications like databases, where the application depends on the state of the previous session.

### Summary
- **Deployment**: Manages identical pods but doesn’t guarantee pod distribution across all nodes.
- **DaemonSet**: Ensures a pod runs on every node in the cluster.
- **StatefulSet**: Manages stateful applications where each pod requires a unique identity and stable storage.

---

## 7. Building an Application with MySQL
We are building an application that requires some specific configurations. First, we’ll need to create a service for MySQL. This will be used to connect the database. Then, we’ll create a storage class for the persistent volume (PV), which is necessary for stateful applications like MySQL. Since MySQL requires persistent storage, we’ll set a volume size of 5GB for each pod.

### Steps We Are Following
1. **Create the Service**: Define a service to expose MySQL. This is a simple YAML file that defines a Kubernetes service of type ClusterIP.
2. **Apply Storage Class**: The storage class allows us to dynamically provision storage, in this case, using AWS’s GP2 volumes.
3. **Create StatefulSet**: This will create MySQL as a stateful set with 3 replicas. Stateful sets manage stateful applications (like databases) and ensure that each pod gets a unique identity and persistent storage. We define a persistent volume claim (PVC) that requests 5GB for each pod.
4. **Check for Issues**: After applying the configuration, sometimes the pods might be in a pending state if the volume provisioning takes time.
5. **Troubleshoot**: If PVCs are still pending, we might need to install the CSI driver (Container Storage Interface driver) for AWS, which helps provision storage volumes in Kubernetes.
6. **Using Kubernetes EFK Stack**: Later, we will deploy an EFK stack (Elasticsearch, FluentD, and Kibana) for logging. This stack is useful for monitoring and logging in Kubernetes clusters. We will create a namespace (EFK log) and apply configurations for FluentD, Elasticsearch, and Kibana.

### Summary
- **Service**: Expose MySQL to the Kubernetes cluster.
- **Persistent Volume**: Storage for stateful applications.
- **StatefulSet**: Ensure MySQL pods maintain state and unique identities.
- **EFK Stack**: For logging and monitoring.

---

## 8. Load Balancer Service
We created a load balancer service in the cloud (you can't do this in a data center) in the Mumbai region. The load balancer type is an Application Load Balancer, and we can access the application through its DNS name. For example, if we open the load balancer’s URL with a port number (e.g., 5601), we can access the Kibana dashboard.

### Setup Overview
1. **Kubernetes Cluster**: Inside the cluster, we have microservices running.
   - **Kibana Deployment**: A simple deployment created for Kibana.
   - **Fluentd**: A log collector that gathers logs from all the nodes and pods and sends them to Elasticsearch.
   - **Elasticsearch Database**: A stateful set running the Elasticsearch database.
2. **Services**:
   - **Internal Communication**: The ClusterIP service is used for communication between Kibana and Elasticsearch.
   - **External Communication**: We use the load balancer service to expose the Kibana application to the internet, allowing us to access the Kibana dashboard.
3. **Fluentd Deployment**: Ensures that logs from all the nodes are collected and sent to Elasticsearch, and Kibana is used to query and monitor those logs.

### Accessing the Application
You can access the application by visiting the load balancer's DNS URL and port, which will take you to the Kibana dashboard.

### Troubleshooting kubectl Issues
If you're getting a "connection to server localhost refused" error, it could be an authentication issue. You may have configured Kubernetes with a specific user (like root), and you need to make sure you're using the right user that has access to the Kubernetes cluster. Check the `~/.kube/config` file for the correct configuration and certificates.

### Notes
- **Namespace**: It's recommended to deploy your application and monitoring services (like Fluentd) in the same namespace. This ensures they are all part of the same application.

---

This Markdown format organizes the content into sections and subsections, making it easier to read and understand. Let me know if you need any further modifications!
