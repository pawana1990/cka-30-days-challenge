# Day 2 – Kubernetes Core Concepts: Pods & kubectl

## 🎯 Objectives

- Understand what a Pod is.
- Understand the relationship between Containers and Pods.
- Deploy a Pod using `kubectl`.
- Inspect Pod details and events.
- Access a running container.
- Troubleshoot Pod issues.

---

## 1. What is a Pod?

A Pod is the smallest deployable unit in Kubernetes.

A Pod can contain one or multiple containers. Containers within the same Pod share networking and can share storage volumes.

### Container vs Pod

| Container | Pod |
|---|---|
| Runs an application process | Kubernetes deployable unit |
| Is managed by a container runtime | Managed by Kubernetes |
| Has its own container filesystem | Provides shared networking to containers in the Pod |
| Cannot be scheduled independently by Kubernetes | Scheduled onto a worker node |

---

## 2. Important kubectl commands

```bash
# Check Kubernetes cluster
kubectl cluster-info

# List nodes
kubectl get nodes -o wide

# List namespaces
kubectl get namespaces

# List pods in current namespace
kubectl get pods

# List pods in all namespaces
kubectl get pods -A

# Create nginx pod
kubectl run nginx --image=nginx

# Detailed pod information
kubectl describe pod nginx

# Check pod logs
kubectl logs nginx

# Access the container
kubectl exec -it nginx -- /bin/sh

# Delete pod
kubectl delete pod nginx
```

---

## 3. Deploy a Pod using YAML

Create a file named `nginx-pod.yaml`.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    app: nginx
spec:
  containers:
    - name: nginx-container
      image: nginx:latest
      ports:
        - containerPort: 80
```

### Deploy the Pod

```bash
kubectl apply -f nginx-pod.yaml
```

### Verify

```bash
kubectl get pods
kubectl get pods -o wide
kubectl describe pod nginx-pod
```

### Delete

```bash
kubectl delete -f nginx-pod.yaml
```

---

## 4. Hands-on labs

### Lab 1 – Create a Pod

```bash
kubectl run web-app --image=nginx
kubectl get pods
```

Expected status: `Running`

### Lab 2 – Inspect the Pod

```bash
kubectl describe pod web-app
```

Identify:
- Pod IP
- Node name
- Container image
- Events
- Current status

### Lab 3 – Check logs

```bash
kubectl logs web-app
```

### Lab 4 – Access container

```bash
kubectl exec -it web-app -- /bin/bash
```

Inside the container:

```bash
hostname
cat /etc/os-release
exit
```

### Lab 5 – Delete and verify

```bash
kubectl delete pod web-app
kubectl get pods
```

---

## 5. Troubleshooting exercise

Create a Pod with an incorrect image:

```bash
kubectl run broken-pod --image=nginx-does-not-exist
```

Check:

```bash
kubectl get pods
kubectl describe pod broken-pod
```

### Questions to investigate

- Why is the Pod not running?
- What does `ErrImagePull` mean?
- What does `ImagePullBackOff` mean?
- Where can you find the failure reason?
- How would you fix the Pod?

Cleanup:

```bash
kubectl delete pod broken-pod
```

---

## 6. Useful commands to remember

```bash
kubectl get pods -o wide
kubectl get pods -w
kubectl describe pod <pod-name>
kubectl logs <pod-name>
kubectl logs <pod-name> --previous
kubectl exec -it <pod-name> -- sh
kubectl delete pod <pod-name>
kubectl get events --sort-by=.metadata.creationTimestamp
```

---

## 7. Mini Quiz

**Q1. What is the smallest deployable unit in Kubernetes?**

**Q2. Can a Pod run multiple containers?**

**Q3. Which command provides detailed information about a Pod?**

**Q4. How can you access a running container inside a Pod?**

**Q5. What is the difference between `kubectl apply` and `kubectl run`?**

**Q6. A Pod is showing `ImagePullBackOff`. What would you investigate?**

### Answers

1. Pod
2. Yes
3. `kubectl describe pod`
4. `kubectl exec -it <pod-name> -- sh`
5. `kubectl apply` manages resources declaratively using manifests or resource definitions, while `kubectl run` is primarily used to create a Pod imperatively.
6. Image name, tag, registry access, credentials, network connectivity, and Pod events.

---

## 8. Day 2 Takeaway

> Kubernetes manages Pods, not standalone containers. Learn to inspect the state, understand events, and troubleshoot before deleting or recreating resources.

---

## 9. Progress Tracker

- [ ] Course lessons completed
- [ ] Pod created using imperative command
- [ ] Pod created using YAML
- [ ] Pod inspected with `describe`
- [ ] Logs checked
- [ ] Container accessed with `exec`
- [ ] Broken Pod troubleshooting completed
- [ ] GitHub notes committed
- [ ] LinkedIn Day 2 post published

---

## Git Commit

```bash
git add .
git commit -m "Day 02: Kubernetes Pods and kubectl hands-on"
git push
```
