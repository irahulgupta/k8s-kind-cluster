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

----
Run kind cluster - with 1 control-plane, 2 worker
$ kind create cluster --name=my-cluster --config=config.yml #replace with whatever your yaml file name
$ kind get cluster
$ kubectl get nodes
ubuntu@ip-172-31-3-46:~/django-notes-app$ kubectl get nodes
NAME                       STATUS   ROLES           AGE   VERSION
my-cluster-control-plane   Ready    control-plane   15m   v1.31.2
my-cluster-worker          Ready    <none>          15m   v1.31.2
my-cluster-worker2         Ready    <none>          15m   v1.31.2

$ docker ps
4c348a0f303f   kindest/node:v1.31.2   "/usr/local/bin/entr…"   16 minutes ago   Up 16 minutes   127.0.0.1:36805->6443/tcp                     my-cluster-control-plane
270fa8312a33   kindest/node:v1.31.2   "/usr/local/bin/entr…"   16 minutes ago   Up 16 minutes                                                 my-cluster-worker
621a19e33aca   kindest/node:v1.31.2   "/usr/local/bin/entr…"   16 minutes ago   Up 16 minutes                                                 my-cluster-worker2

#clone any repo with one tier application
$ git clone https://github.com/LondheShubham153/django-notes-app.git
$ git checkout dev

#check docker file
$ docker build -t notesapp .
$ docker images
$ docker run -d -p 8000:8000 notesapp:latest
$ docker ps
CONTAINER ID   IMAGE                  COMMAND                  CREATED          STATUS          PORTS                                         NAMES
ebd31824d279   notesapp:latest        "/bin/sh -c 'python …"   5 minutes ago    Up 5 minutes    0.0.0.0:8000->8000/tcp, [::]:8000->8000/tcp   jolly_hofstadter
4c348a0f303f   kindest/node:v1.31.2   "/usr/local/bin/entr…"   16 minutes ago   Up 16 minutes   127.0.0.1:36805->6443/tcp                     my-cluster-control-plane
270fa8312a33   kindest/node:v1.31.2   "/usr/local/bin/entr…"   16 minutes ago   Up 16 minutes                                                 my-cluster-worker
621a19e33aca   kindest/node:v1.31.2   "/usr/local/bin/entr…"   16 minutes ago   Up 16 minutes                                                 my-cluster-worker2

#add the security group 8000:8000
#check if your nodesapp is working or not: http://54.163.48.57:8000/

#dockerhub- create personal access token- login back to terminal with loginid and personal token
$docker images
$docker image tag notesapp:latest irahulgupta/notesapp:latest
$docker push irahulgupta/notesapp:latest

#create new folder name as 'k8s' inside that folder create deployment.yml, namespace.yml, service.yml

ubuntu@ip-172-31-3-46:~/django-notes-app$ cd k8s
ubuntu@ip-172-31-3-46:~/django-notes-app/k8s$ ls
deployment.yml  namespace.yml  pod.yml  service.yml
ubuntu@ip-172-31-3-46:~/django-notes-app/k8s$ rm pod.yml
ubuntu@ip-172-31-3-46:~/django-notes-app/k8s$ ls
deployment.yml  namespace.yml  service.yml
ubuntu@ip-172-31-3-46:~/django-notes-app/k8s$ vim deployment
ubuntu@ip-172-31-3-46:~/django-notes-app/k8s$ ls
deployment.yml  namespace.yml  service.yml
ubuntu@ip-172-31-3-46:~/django-notes-app/k8s$ vim deployment.yml
ubuntu@ip-172-31-3-46:~/django-notes-app/k8s$ vim namespace.yml
ubuntu@ip-172-31-3-46:~/django-notes-app/k8s$ vim service.yml
ubuntu@ip-172-31-3-46:~/django-notes-app/k8s$ cat service.yml
apiVersion: v1
kind: Service
metadata:
  name: notesapp-service
  namespace: notesapp
spec:
  selector:
    app: notes-app
  ports:
    - protocol: TCP
      port: 8000
      targetPort: 8000
  type: NodePort
ubuntu@ip-172-31-3-46:~/django-notes-app/k8s$ cat namespace.yml
kind: Namespace
apiVersion: v1
metadata:
  name: notesapp
ubuntu@ip-172-31-3-46:~/django-notes-app/k8s$ cat deployment.yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: notesapp-deployment
  namespace: notesapp
  labels:
    app: notes-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: notes-app
  template:
    metadata:
      labels:
        app: notes-app
    spec:
      containers:
        - name: notes-app
          image: irahulgupta/notesapp:latest
          ports:
            - containerPort: 8000

ubuntu@ip-172-31-3-46:~/django-notes-app/k8s$ kubectl apply -f .
namespace/notesapp created
service/notesapp-service created
Error from server (BadRequest): error when creating "deployment.yml": Deployment in version "v1" cannot be handled as a Deployment: strict decoding error: unknown field "spec.template.spec.containers[1].containerPort"
ubuntu@ip-172-31-3-46:~/django-notes-app/k8s$ kubectl apply -f .
namespace/notesapp unchanged
service/notesapp-service unchanged
Error from server (BadRequest): error when creating "deployment.yml": Deployment in version "v1" cannot be handled as a Deployment: strict decoding error: unknown field "spec.template.spec.containers[1].containerPort"
ubuntu@ip-172-31-3-46:~/django-notes-app/k8s$ vim deployment.yml
ubuntu@ip-172-31-3-46:~/django-notes-app/k8s$ cat deployment.yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: notesapp-deployment
  namespace: notes
  labels:
    app: notes-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: notes-app
  template:
    metadata:
      labels:
        app: notes-app
    spec:
      containers:
        - name: notes-app
          image: irahulgupta/notesapp:latest
          ports:
            - containerPort: 8000

ubuntu@ip-172-31-3-46:~/django-notes-app/k8s$ kubectl apply -f .
deployment.apps/notesapp-deployment created
namespace/notesapp unchanged
service/notesapp-service unchanged

ubuntu@ip-172-31-3-46:~/django-notes-app/k8s$ kubectl get all -n notesapp
NAME                                       READY   STATUS             RESTARTS   AGE
pod/notesapp-deployment-55868d77f6-8cn52   0/1     ErrImagePull       0          2m36s
pod/notesapp-deployment-55868d77f6-9cx9t   0/1     ImagePullBackOff   0          2m36s
pod/notesapp-deployment-55868d77f6-zfdvv   0/1     ImagePullBackOff   0          2m36s

NAME                       TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
service/notesapp-service   NodePort   10.96.173.122   <none>        8000:30835/TCP   7m14s

NAME                                  READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/notesapp-deployment   0/3     3            0           2m36s

NAME                                             DESIRED   CURRENT   READY   AGE
replicaset.apps/notesapp-deployment-55868d77f6   3         3         0       2m36s

#Now use port forward command
ubuntu@ip-172-31-3-46:~/django-notes-app/k8s$ kubectl port-forward service/notesapp-service -n notesapp 8000:8000 --address=0.0.0.0