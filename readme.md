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

# Installation Guide

## 1. Install Docker

```bash
sudo apt update
sudo apt install -y docker.io
sudo systemctl start docker
sudo systemctl enable docker
```

Verify:

```bash
docker --version
```

---

## 2. Install kubectl

```bash
curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.19.6/2021-01-05/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin
kubectl version --short --client
```

```bash
kubectl version --client
```

---

## 3. Install Kind (Kubernetes in Docker)

```bash
curl -Lo ./kind https://kind.sigs.k8s.io/dl/latest/kind-linux-amd64

chmod +x kind
sudo mv kind /usr/local/bin/
```

Verify:

```bash
kind --version
```

---

## 4. Create Kubernetes Cluster

```bash
kind create cluster
```

Check cluster:

```bash
kubectl get nodes
```

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
$ kubectl port-forward service/notes-app-service -n notes 8000:8000 --address=0.0.0.0



