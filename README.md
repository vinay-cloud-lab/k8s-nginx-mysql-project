# k8s-nginx-mysql-project
Kubernetes project showcasing Nginx deployment with Ingress, NetworkPolicies for secure traffic control, and MySQL StatefulSet with persistent storage.


# Kubernetes DevOps Project: Nginx, NetworkPolicy & MySQL StatefulSet

## 📌 Overview

This project demonstrates real-world Kubernetes DevOps practices including application deployment, secure networking, and persistent storage management.

It simulates a production-like setup with:

* Nginx exposed via Ingress
* NetworkPolicies for traffic control and security
* MySQL StatefulSet with persistent storage
* Data retention across pod restarts

---

## 📁 Project Structure

* **nginx-ingress/** → Nginx deployment, service, and ingress configuration
* **network-policy/** → Security rules for traffic control between pods
* **mysql-statefulset/** → MySQL database with persistent storage setup

---

## 🚀 Nginx + Ingress Setup

### Components:

* Namespace: `nginx-namespace`
* Deployment: Nginx (1 replica)
* Service: ClusterIP
* Ingress: External access configuration

### Purpose:

Expose Nginx application externally using Ingress Controller.

---

## 🔐 Network Security (NetworkPolicy)

### Components:

* Default deny-all policy
* Allow rule for specific workloads
* Test pods for validation:

  * `allowed-pod`
  * `denied-pod`

### Purpose:

Demonstrates Kubernetes network isolation and controlled communication between workloads.

---

## 💾 MySQL StatefulSet with Persistent Storage

### Components:

* StorageClass
* PersistentVolume (PV)
* PersistentVolumeClaim (PVC)
* Secret for database credentials
* MySQL StatefulSet
* Service for internal access

### Database Operations:

```sql id="mysql01"
CREATE DATABASE kubedb;
USE kubedb;

CREATE TABLE users (
  id INT,
  name VARCHAR(50)
);

INSERT INTO users VALUES (1, 'devops');
SELECT * FROM users;
```

---

## ⚙️ Deployment Steps

```bash id="apply01"
kubectl apply -f nginx-ingress/
kubectl apply -f network-policy/
kubectl apply -f mysql-statefulset/
```

---

## 🧪 Verification

### 🌐 Nginx Access Test

```bash id="nginx01"
kubectl exec -n pod-namespace -it allowed-pod -- wget -qO- http://nginx-service.nginx-namespace
```

Expected:

* Accessible from allowed pod

---

### 🚫 NetworkPolicy Test

```bash id="net01"
kubectl exec -n pod-namespace -it denied-pod -- wget -qO- http://nginx-service.nginx-namespace
```

Expected:

* Request blocked

---

### 🗄️ MySQL Access

```bash id="db01"
kubectl exec -it <mysql-pod> -- mysql -u root -p
```

---

## 🔁 Data Persistence Test

1. Delete StatefulSet:

```bash id="del01"
kubectl delete statefulset mysql
```

2. Recreate:

```bash id="recreate01"
kubectl apply -f mysql-statefulset/
```

3. Verify data remains:

```sql id="verify01"
USE kubedb;
SELECT * FROM users;
```

---

## ⚠️ Prerequisites

* Kubernetes cluster (Minikube / EKS / AKS / GKE)
* kubectl configured
* Ingress Controller installed (e.g., NGINX Ingress Controller)

---

## 📚 Key DevOps Concepts Covered

* Kubernetes Deployments & Services
* Ingress-based external exposure
* Network segmentation using NetworkPolicies
* Stateful workloads with persistent storage
* Data persistence across pod lifecycle
* Secret management

---

## 🎯 Outcome

This project demonstrates a production-style Kubernetes setup focusing on application exposure, security enforcement, and stateful data management using best practices.
