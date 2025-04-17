vi pod.yaml (#name of the file)

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
spec:
  containers:
  - name: nginx
    image: nginx:latest
    ports:
    - containerPort: 80
```

```bash
kubctl apply -f mypod.yaml
```
```bash
kubctl get pod
```
```bash
kubctl delete pod nginx-pod
```
```bash
kubctl get pod
```


