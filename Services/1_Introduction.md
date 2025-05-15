### Understanding Services and Labels in Kubernetes

In the previous section, we launched our first Pod, which runs a web container on port 80. We then attempted—perhaps a bit naively—to access that container via a web browser. Unsurprisingly, this attempt failed. That’s because Pods are **not accessible outside the Kubernetes cluster by default**. This behavior is by design.

Pods in Kubernetes are **ephemeral** by nature—they're meant to be short-lived, disposable entities. They can be terminated and recreated frequently. As the saying goes in cloud-native environments: *treat your servers like cattle, not pets*. Because of this transient lifecycle, exposing Pods directly to the outside world isn't practical or reliable.

To solve this problem, Kubernetes provides a higher-level abstraction called a **Service**.

---

### What is a Kubernetes Service?

A **Service** is a long-lived, stable object within Kubernetes that acts as a reliable network endpoint. Each Service gets:

* A **stable IP address**
* A **consistent port**
* And most importantly, it **automatically routes traffic to the appropriate Pod(s)** behind the scenes.

By attaching a Service to a Pod, you make that Pod accessible—either within the cluster or even externally, depending on the type of Service used.

---

### How Does a Service Find a Pod?

This is where **labels** and **selectors** come in.

* **Labels** are key-value pairs that you assign to Kubernetes objects like Pods.

  * Example: `app: webapp`
* A **selector** is how a Service identifies which Pods to route traffic to.

  * Example: The Service will select all Pods with the label `app=webapp`.

At runtime, the Service continuously looks for Pods that match its selector criteria. When it finds them, it routes incoming traffic to those Pods. This mechanism is both powerful and flexible, allowing you to scale and manage services dynamically.

---

### Quick Example

Let’s say we have a Pod with this label:

```yaml
labels:
  app: webapp
```

We then define a Service with the following selector:

```yaml
selector:
  app: webapp
```

Kubernetes will ensure that traffic to the Service is directed to any Pod with the matching label.

---

### Let’s Get Practical

We’ll now create a Service for our `webapp` Pod. But before that, if you’ve restarted your Minikube cluster, it’s a good idea to check the current state using:

```bash
kubectl get all
```

You might notice that the `webapp` Pod is still running, even after a restart. Kubernetes has remembered the previous state, although the Pod may have been restarted.

You can view more details using:

```bash
kubectl describe pod webapp
```

---

### Creating the Service YAML File

Let’s write our first Service definition in a file called `webapp-service.yaml`. Here’s what a basic version might look like:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: webapp-service
spec:
  selector:
    app: webapp
  ports:
    - protocol: TCP
      port: 80       # The port exposed by the Service
      targetPort: 80 # The port on the Pod
  type: ClusterIP    # We'll discuss this in more detail later
```

* `apiVersion`, `kind`, and `metadata` are familiar from when we created Pods.
* `selector` links this Service to the matching Pods.
* `ports` define how traffic should be routed.
* `type` defines the accessibility of the Service (e.g., `ClusterIP`, `NodePort`, `LoadBalancer`).

We’ll start with `ClusterIP`, which exposes the Service only within the cluster. Later, we’ll explore how to expose Services externally using other types.

---

In the next section, we’ll apply this configuration and test connectivity to the Pod via the Service. Then, we’ll move on to making Services available outside the cluster.

Let’s move to our editor and get started.

---
