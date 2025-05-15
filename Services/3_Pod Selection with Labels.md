## 🧵 Understanding Kubernetes Services, Selectors, and Labels – With Zero Downtime Deployment Strategy

### 🔹 What went wrong initially?

When the service was first deployed, it didn’t work. The root cause was the **lack of labels** on the pod. Services in Kubernetes use **label selectors** to find matching pods. If no matching labels are found on any pods, the service cannot forward traffic.

---

### 🔹 Fixing the issue with labels

* Labels were missing in the original pod definition.
* A label was added in the pod metadata:

  ```yaml
  labels:
    app: webapp
  ```
* The corresponding service was using:

  ```yaml
  selector:
    app: webapp
  ```
* ✅ Once the pod and service were updated using:

  ```bash
  kubectl apply -f pod.yaml
  kubectl apply -f service.yaml
  ```

  the service correctly routed traffic to the pod.

---

### 🔹 Labels are arbitrary

* Labels like `app: webapp` are **not required names**, just a common convention.
* You can name them anything (e.g., `mylabel: webapp`), as long as the **selector matches** the label.

---

### 🔹 Introducing release versioning

To support **zero downtime deployments**, labels are again very useful.

1. Add an **extra label** to each pod to indicate the release:

   ```yaml
   labels:
     app: webapp
     release: "0"
   ```

   and for the second pod:

   ```yaml
   labels:
     app: webapp
     release: "0-5"
   ```

2. Modify the service selector to:

   ```yaml
   selector:
     app: webapp
     release: "0"
   ```

   This ensures the service routes traffic **only** to the matching release.

---

### 🔄 Performing a Zero Downtime Update

1. Define a second pod (new release) in the same YAML file using `---` as a separator.

2. Apply the updated YAMLs:

   ```bash
   kubectl apply -f pods.yaml
   kubectl apply -f service.yaml
   ```

   This will launch the new pod **in parallel** without affecting the current service.

3. Once the new pod is ready, **switch the service selector**:

   ```yaml
   selector:
     app: webapp
     release: "0-5"
   ```

   Reapply the service:

   ```bash
   kubectl apply -f service.yaml
   ```

   This switch is almost **instant**, as only the service routing changes—not the pod creation.

---

### 🧪 Useful Debugging and Inspection Commands

* See all resources:

  ```bash
  kubectl get all
  ```

* See all pods:

  ```bash
  kubectl get pods      # or get po
  ```

* Show labels:

  ```bash
  kubectl get po --show-labels
  ```

* Filter by label:

  ```bash
  kubectl get po -l release=0
  kubectl get po -l release=0-5
  ```

* Describe a service:

  ```bash
  kubectl describe service <name>   # or svc for short
  ```

---

### ✅ Summary

* **Labels and selectors** are powerful tools for routing traffic in Kubernetes.
* They allow **flexible**, **safe**, and **zero-downtime deployments**.
* You can control exactly **which pods** a service routes to by updating selectors.
* This pattern is a precursor to **rolling updates** and **canary deployments**, which will be covered in later chapters.
