## Kubernetes Cluster with Minikube

[![Kubernetes](https://img.shields.io/badge/Kubernetes-1.28-blue)](https://kubernetes.io/)
[![Minikube](https://img.shields.io/badge/Minikube-1.38-green)](https://minikube.sigs.k8s.io/)
[![Docker](https://img.shields.io/badge/Docker-Desktop-blue)](https://www.docker.com/)
[![Status](https://img.shields.io/badge/Status-Completed-brightgreen)]()

## 📝 Task Overview

**Objective:** Deploy and manage applications in Kubernetes using Minikube.

### Requirements Completed:
- ✅ Install Minikube and start cluster
- ✅ Create `deployment.yaml` for Nginx app
- ✅ Create `service.yaml` to expose app
- ✅ Verify using `kubectl get pods` and `kubectl get services`
- ✅ Scale deployment using `kubectl scale`
- ✅ Debug using `kubectl describe` and logs

---

## 🛠️ Technologies Used

| Technology | Version | Purpose |
|------------|---------|---------|
| **Minikube** | v1.38.1 | Local Kubernetes cluster |
| **kubectl** | v1.28.0 | Kubernetes CLI management |
| **Docker Desktop** | Latest | Container runtime |
| **Nginx** | Latest | Demo application |
| **Windows 10** | 22H2 | Host OS |

---

## 🏗️ Architecture

\`\`\`
┌─────────────────────────────────────────────────────────┐
│                    WINDOWS 10 LAPTOP                    │
│                                                         │
│  ┌───────────────────────────────────────────────────┐ │
│  │           MINIKUBE KUBERNETES CLUSTER             │ │
│  │                                                   │ │
│  │  ┌─────────────────────────────────────────────┐  │ │
│  │  │         DEPLOYMENT: nginx-deployment        │  │ │
│  │  │            Replicas: 3                      │  │ │
│  │  │                                             │  │ │
│  │  │  ┌─────┐  ┌─────┐  ┌─────┐                │  │ │
│  │  │  │Pod 1│  │Pod 2│  │Pod 3│                │  │ │
│  │  │  │Nginx│  │Nginx│  │Nginx│                │  │ │
│  │  │  │:80  │  │:80  │  │:80  │                │  │ │
│  │  │  └──┬──┘  └──┬──┘  └──┬──┘                │  │ │
│  │  │     └────────┼────────┘                    │  │ │
│  │  │              ▼                             │  │ │
│  │  │      ┌──────────────┐                      │  │ │
│  │  │      │SERVICE       │                      │  │ │
│  │  │      │NodePort:30000│                      │  │ │
│  │  │      └──────┬───────┘                      │  │ │
│  │  └─────────────┼─────────────────────────────┘  │ │
│  │                ▼                                 │ │
│  │         http://192.168.49.2:30000               │ │
│  └───────────────────────────────────────────────────┘ │
│                         ▼                               │
│                   BROWSER ACCESS                        │
└─────────────────────────────────────────────────────────┘
\`\`\`

---

## 📄 Files Description

### 1. `deployment.yaml`
Defines the application deployment configuration.

\`\`\`yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
        resources:
          limits:
            memory: "128Mi"
            cpu: "100m"
\`\`\`

### 2. `service.yaml`
Exposes the application to external traffic.

\`\`\`yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
  labels:
    app: nginx
spec:
  type: NodePort
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
      nodePort: 30000
\`\`\`

---

## 💻 Setup Instructions

### Prerequisites
\`\`\`bash
# Verify Docker is running
docker --version

# Verify Minikube installation
minikube version

# Verify kubectl installation
kubectl version --client
\`\`\`

### Start Kubernetes Cluster
\`\`\`bash
# Start Minikube with Docker driver
minikube start --driver=docker

# Check cluster status
minikube status
kubectl cluster-info
kubectl get nodes
\`\`\`

---

## 🚀 Deployment Steps

### Step 1: Apply Configurations
\`\`\`bash
# Deploy the application
kubectl apply -f deployment.yaml

# Expose the service
kubectl apply -f service.yaml
\`\`\`

### Step 2: Verify Deployment
\`\`\`bash
# Check pods status
kubectl get pods

# Check services
kubectl get services

# Check deployments
kubectl get deployments
\`\`\`

### Step 3: Access the Application
\`\`\`bash
# Get the access URL
minikube service nginx-service --url

# Open in browser
minikube service nginx-service
\`\`\`

**Access URL:** `http://192.168.49.2:30000`

---

## 📊 Results & Verification

### Pod Status
\`\`\`bash
$ kubectl get pods
NAME                                READY   STATUS    RESTARTS   AGE
nginx-deployment-74d5957bf9-46vk7   1/1     Running   0          58s
nginx-deployment-74d5957bf9-hv8ct   1/1     Running   0          4m47s
nginx-deployment-74d5957bf9-p6hdv   1/1     Running   0          58s
\`\`\`

### Service Status
\`\`\`bash
$ kubectl get services
NAME            TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
kubernetes      ClusterIP   10.96.0.1      <none>        443/TCP        12m
nginx-service   NodePort    10.111.79.51   <none>        80:30000/TCP   4m41s
\`\`\`

### Deployment Status
\`\`\`bash
$ kubectl get deployments
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deployment   3/3     3            3           4m47s
\`\`\`

---

## 📈 Scaling Demonstration

### Scale from 1 to 3 Replicas
\`\`\`bash
# Scale the deployment
kubectl scale deployment nginx-deployment --replicas=3

# Verify scaling
kubectl get pods

# Watch scaling in real-time
kubectl get pods --watch
\`\`\`

### Scaling Results
| Metric | Before Scaling | After Scaling |
|--------|---------------|---------------|
| **Replicas** | 1 | 3 |
| **Available Pods** | 1 | 3 |
| **Load Capacity** | 1x | 3x |
| **High Availability** | ❌ No | ✅ Yes |

---

## 🔍 Debugging & Logs

### Pod Details
\`\`\`bash
# Get detailed pod information
kubectl describe pod nginx-deployment-74d5957bf9-hv8ct
\`\`\`

**Key Information:**
- **Status:** Running
- **Node:** minikube/192.168.49.2
- **IP Address:** 10.244.0.4
- **Container ID:** docker://044fdc822e8...
- **Image:** nginx:latest
- **Restart Count:** 0

### Container Logs
\`\`\`bash
# View pod logs
kubectl logs nginx-deployment-74d5957bf9-hv8ct

# Stream logs in real-time
kubectl logs -f nginx-deployment-74d5957bf9-hv8ct
\`\`\`

### Events
\`\`\`bash
# View cluster events
kubectl get events --sort-by='.lastTimestamp'
\`\`\`

---

## 📚 Commands Reference

\`\`\`bash
# Cluster Management
minikube start --driver=docker
minikube stop
minikube status
minikube delete

# Deployment Commands
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
kubectl get pods
kubectl get services
kubectl get deployments

# Scaling
kubectl scale deployment nginx-deployment --replicas=3

# Debugging
kubectl describe pod <pod-name>
kubectl logs <pod-name>
kubectl get events

# Cleanup
kubectl delete deployment nginx-deployment
kubectl delete service nginx-service
\`\`\`

---






---
**⭐ Star this repository if you found it helpful!**
"@ | Out-File -FilePath "README.md" -Encoding utf8
