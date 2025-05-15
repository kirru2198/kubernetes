### 🚀 Writing a Kubernetes Service for `fleetman-webapp`

Let’s create a Kubernetes Service to expose our web application (`fleetman-webapp`). We'll write the YAML file from scratch, explaining each section as we go.

---

#### 🔹 **YAML Definition**

```yaml
apiVersion: v1
kind: Service
metadata:
  name: fleetman-webapp
spec:
  selector:
    app: webapp  # This matches the label of the target Pod(s)
  ports:
    - name: http
      protocol: TCP
      port: 80         # Port exposed by the service
      targetPort: 80   # Port on the Pod the traffic will go to
      nodePort: 30080  # External port to access the service (only valid for NodePort)
  type: NodePort       # Exposes the service on a port on each Node
```

---

### 🧠 Explanation

#### ✅ `apiVersion` and `kind`

* We’re using the core `v1` API.
* The `kind` is `Service`, which tells Kubernetes we are defining a service resource.

#### ✅ `metadata`

* We name the service `fleetman-webapp` to follow a consistent naming convention:
  `fleetman-<microservice-name>`

#### ✅ `spec.selector`

* The selector is **how the Service knows which Pod(s) to forward traffic to**.
* It looks for Pods with the label `app: webapp`.
* This is like saying, *"Only send traffic to Pods that are part of this web application."*

  > Think of labels as name tags, and the Service is saying:
  > "I only talk to Pods with the tag `app=webapp`."

#### ✅ `ports`

* Defines how the Service listens for and forwards traffic:

  * `port`: the port the Service will listen on (inside the cluster).
  * `targetPort`: the port the Pod is listening on.
  * `nodePort`: the external port exposed to users (required only for `NodePort` type).
* In Docker, this is similar to `-p 30080:80`, meaning we’re exposing 30080 on the host and forwarding it to 80 inside the container.

#### ✅ `type: NodePort`

* Makes the Service accessible externally via `<NodeIP>:<NodePort>` (e.g., `192.168.99.100:30080`).
* NodePort ranges must be **between 30000–32767**. We chose `30080`.

> ⚠️ This setup is great for local development with Minikube or bare-metal clusters.
> In cloud environments, you'd use `LoadBalancer` or `Ingress`.

---

### 🧪 Applying the YAML

Once your Service YAML is ready, apply it with:

```bash
kubectl apply -f fleetman-webapp-service.yaml
```

To check the status:

```bash
kubectl get services
```

You’ll see output like this:

```
NAME               TYPE       CLUSTER-IP     EXTERNAL-IP   PORT(S)           AGE
fleetman-webapp    NodePort   10.96.0.1      <none>        80:30080/TCP      1m
```

> `CLUSTER-IP`: Used for internal communication
> `EXTERNAL-IP`: Will remain `<none>` unless you're using a `LoadBalancer`
> `PORT(S)`: Shows internal port : external port mapping

---

### 🌐 Accessing the Web App

To access your web app locally via Minikube:

```bash
minikube ip
```

Let’s say it returns `192.168.99.100`, then open:

```
http://192.168.99.100:30080
```

If the site doesn’t load, ensure:

* The Pod is running.
* The Pod has the correct label (`app: webapp`).
* The port inside the container is actually 80.

> 🧩 In the video/course, one crucial step was deliberately skipped. That’s likely the Pod or Deployment definition itself — without that, there’s nothing for the Service to route to!

---
