# Kubernetes Nginx Demo (Kind Cluster)

## 📌 Overview

This project sets up a simple **Kubernetes cluster using Kind** and deploys an **Nginx application** with:

* Namespace
* Pod
* Deployment
* Service (NodePort)

---

## 📁 Project Structure

```
k8s-practice/
│── config.yml          # Kind cluster configuration
│── ngnix/
    │── namespace.yml   # Namespace definition
    │── pod.yml         # Single Nginx pod
    │── deployment.yml  # Nginx deployment (3 replicas)
    │── service.yml     # NodePort service
```

---

## ⚙️ Prerequisites

* Docker installed
* Kind installed
* kubectl installed

---

## 🚀 Setup Steps

### 1. Create Kind Cluster

```bash
kind create cluster --config=config.yml
```

### 2. Create Namespace

```bash
kubectl apply -f ngnix/namespace.yml
```

### 3. Deploy Resources

```bash
kubectl apply -f ngnix/
```

---

## 🔍 Verify Resources

```bash
kubectl get all -n nginx
```

---

## 🌐 Access Application

Get NodePort:

```bash
kubectl get svc -n nginx
```

Then access in browser:

```
http://<Node-IP>:<NodePort>
```

---

## 🧠 Notes

* Deployment runs **3 replicas** of Nginx.
* Service type is **NodePort** for external access.
* Namespace helps isolate resources.

---

## ✅ Summary

This is a basic Kubernetes setup to understand:

* Cluster creation using Kind
* Resource management using YAML
* Deploying and exposing an application

---
