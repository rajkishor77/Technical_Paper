# ☸️ Kubernetes (K8s) — Complete Study Notes
> **After reading this, every Kubernetes concept will be crystal clear.**
> Last Updated: May 2026 | Research-backed & Production-ready

---

## 📌 Table of Contents
1. [What is Kubernetes?](#1-what-is-kubernetes)
2. [Why Kubernetes? — The Problem It Solves](#2-why-kubernetes--the-problem-it-solves)
3. [Monolithic vs Microservices](#3-monolithic-vs-microservices)
4. [Kubernetes Architecture — Deep Dive](#4-kubernetes-architecture--deep-dive)
5. [Control Plane Components](#5-control-plane-components)
6. [Worker Node Components](#6-worker-node-components)
7. [Core Kubernetes Objects](#7-core-kubernetes-objects)
8. [Pod — In Depth](#8-pod--in-depth)
9. [Deployments & ReplicaSets](#9-deployments--replicasets)
10. [Services & Networking](#10-services--networking)
11. [Ingress](#11-ingress)
12. [ConfigMap & Secrets](#12-configmap--secrets)
13. [Volumes & Persistent Storage](#13-volumes--persistent-storage)
14. [Namespaces](#14-namespaces)
15. [Scheduling — Taints, Tolerations, Affinity](#15-scheduling--taints-tolerations-affinity)
16. [Deployment Strategies](#16-deployment-strategies)
17. [Probes — Liveness, Readiness, Startup](#17-probes--liveness-readiness-startup)
18. [StatefulSets & DaemonSets](#18-statefulsets--daemonsets)
19. [Resource Quotas & Limits](#19-resource-quotas--limits)
20. [RBAC — Role-Based Access Control](#20-rbac--role-based-access-control)
21. [Helm — Kubernetes Package Manager](#21-helm--kubernetes-package-manager)
22. [All kubectl Commands — Explained](#22-all-kubectl-commands--explained)
23. [Best Practices](#23-best-practices)
24. [Kubernetes vs Docker Swarm](#24-kubernetes-vs-docker-swarm)
25. [Common Interview Questions](#25-common-interview-questions)

---

## 1. What is Kubernetes?

Kubernetes (K8s) is an **open-source container orchestration platform** that automates the deployment, scaling, and management of containerized applications across a cluster of machines.

- **Origin:** Developed by Google (inspired by internal systems Borg and Omega)
- **Released:** 2014 (open-sourced)
- **Donated to:** Cloud Native Computing Foundation (CNCF) in 2015
- **Name meaning:** Greek for "helmsman" or "pilot" — it steers your containers
- **Abbreviation:** K8s → K + 8 letters + s

### Adoption in 2026
- **82%** of container users run Kubernetes in production (CNCF Survey 2025)
- **80%+** of senior cloud-native job listings require Kubernetes knowledge
- **66%** of organizations deploying generative AI models rely on Kubernetes

### Simple Analogy — The Orchestra Conductor 🎼
> - Each **container** = a musician in an orchestra
> - You can manage 3–5 musicians yourself, but not 500
> - **Kubernetes** = the conductor who coordinates the entire orchestra
> - You hand the conductor the **sheet music** (your YAML config = desired state)
> - The conductor ensures every musician plays correctly, replaces sick players automatically, and adds more players when the crowd grows

---

## 2. Why Kubernetes? — The Problem It Solves

### Before Kubernetes
When apps scaled to hundreds or thousands of containers, these problems appeared:

| Problem | Without Kubernetes |
|---------|-------------------|
| **Scalability** | Manual scaling; someone had to `docker run` every new instance |
| **Self-healing** | A crashed container stayed dead until someone noticed |
| **Load Balancing** | Manual load balancer configuration for every service |
| **Rolling Updates** | Downtime required to update — stop, update, restart |
| **Multi-node management** | Tracking which container runs on which server — chaos |
| **Resource utilization** | Servers either over-provisioned or running out of memory |
| **Service discovery** | Hardcoded IPs broke whenever containers restarted |

### What Kubernetes Provides
- **Automated Scheduling** — places containers on the right node based on available resources
- **Self-Healing** — restarts crashed containers, replaces failed nodes, kills unhealthy containers
- **Horizontal Scaling** — scale up/down with one command or automatically based on CPU/memory
- **Rolling Updates & Rollbacks** — deploy new versions with zero downtime; roll back instantly
- **Service Discovery & Load Balancing** — stable internal DNS; traffic distributed across healthy pods
- **Secret & Config Management** — separate config and secrets from application code
- **Storage Orchestration** — automatically mount storage from local disk, cloud, NFS, etc.

---

## 3. Monolithic vs Microservices

### Monolithic Architecture
```
┌─────────────────────────────────────────┐
│           Single Large Application       │
│  ┌──────┐ ┌─────────┐ ┌──────────────┐  │
│  │  UI  │ │Payments │ │Notifications │  │
│  └──────┘ └─────────┘ └──────────────┘  │
│  ┌──────┐ ┌─────────┐ ┌──────────────┐  │
│  │Search│ │  Auth   │ │   Orders     │  │
│  └──────┘ └─────────┘ └──────────────┘  │
└─────────────────────────────────────────┘
         One deployment unit
```
**Problem:** Change one module → redeploy entire app → one bug crashes everything.

### Microservices Architecture
```
[UI Service] → [API Gateway] → [Auth Service]
                             → [Payment Service]
                             → [Order Service]
                             → [Notification Service]
                             → [Search Service]

Each service = independent container, deployed separately
```
**Result:** Update Payment Service → only that service redeployed. Others unaffected.

**The New Problem:** 500 containers to manage → need an orchestrator → **Kubernetes**.

---

## 4. Kubernetes Architecture — Deep Dive

A Kubernetes cluster has two layers: the **Control Plane** (brain) and **Worker Nodes** (muscle).

```
┌─────────────────────────────────────────────────────────────────────┐
│                         CONTROL PLANE (Master)                       │
│                                                                      │
│  ┌──────────────┐  ┌────────┐  ┌──────────────────┐  ┌──────────┐  │
│  │  kube-api    │  │  etcd  │  │  kube-scheduler   │  │Controller│  │
│  │   server     │  │ (DB)   │  │  (placement)      │  │ Manager  │  │
│  └──────┬───────┘  └────────┘  └──────────────────┘  └──────────┘  │
│         │ (only component that talks to etcd)                        │
└─────────┼───────────────────────────────────────────────────────────┘
          │  HTTPS / TLS
          │ (kubelet polls API server)
┌─────────┼───────────────────────────────────────────────────────────┐
│         │          WORKER NODES (Data Plane)                         │
│  ┌──────┴──────────────────────────────────────────────────┐        │
│  │  Node 1              Node 2              Node 3          │        │
│  │  ┌─────────┐         ┌─────────┐         ┌─────────┐    │        │
│  │  │ kubelet │         │ kubelet │         │ kubelet │    │        │
│  │  │kube-    │         │kube-    │         │kube-    │    │        │
│  │  │ proxy   │         │ proxy   │         │ proxy   │    │        │
│  │  │ Pod A   │         │ Pod B   │         │ Pod C   │    │        │
│  │  │ Pod D   │         │ Pod E   │         │ Pod F   │    │        │
│  │  └─────────┘         └─────────┘         └─────────┘    │        │
│  └─────────────────────────────────────────────────────────┘        │
└─────────────────────────────────────────────────────────────────────┘
```

### The Flow of a `kubectl apply` Command
```
You run: kubectl apply -f deployment.yaml
          ↓
1. kubectl sends HTTPS request to kube-apiserver
          ↓
2. apiserver authenticates + authorizes the request (RBAC)
          ↓
3. apiserver validates the YAML and stores intent in etcd
          ↓
4. kube-scheduler watches apiserver for unscheduled pods
          ↓
5. scheduler picks the best node (checks resources, taints, affinity)
          ↓
6. scheduler writes node assignment to apiserver → stored in etcd
          ↓
7. kubelet on chosen node polls apiserver, sees new pod spec
          ↓
8. kubelet tells container runtime (containerd) to pull image + start container
          ↓
9. kubelet reports pod status back to apiserver → stored in etcd
          ↓
10. Your pod is running ✅
```

---

## 5. Control Plane Components

### 5.1 kube-apiserver — The Front Door
- **Central hub** of the entire cluster — every request goes through it
- Exposes the Kubernetes REST API over HTTPS
- Handles: **Authentication** → **Authorization (RBAC)** → **Admission Control** → **Validation** → **Persists to etcd**
- The **ONLY** component that directly reads/writes to etcd
- All other components (scheduler, controller manager, kubelet) connect to the API server — never to each other directly
- Scales horizontally — multiple instances can run behind a load balancer for HA
- Think of it as: **the receptionist of a building — every request must go through here**

```
kubectl / Helm / CI tool
        ↓
   kube-apiserver  →  etcd (only apiserver writes here)
   ↑       ↑
scheduler  controller-manager  (watch apiserver for changes)
           ↑
         kubelet (on each node)
```

### 5.2 etcd — The Cluster Database
- A **distributed, strongly-consistent key-value store**
- Stores the **entire state of the cluster**: all objects, desired state, actual state, secrets, configs
- The **single source of truth** for Kubernetes
- Built on Raft consensus algorithm — fault-tolerant
- If etcd goes down: cluster state cannot be changed (existing workloads keep running but no new scheduling)
- **Always backup etcd in production!**

```
etcd stores:
  - Pod definitions         - Node information
  - Deployments             - ConfigMaps and Secrets
  - Service definitions     - RBAC policies
  - Cluster configuration   - Namespace definitions
```

**Fault tolerance:**
- 3-node etcd → survives 1 node failure
- 5-node etcd → survives 2 node failures
- Formula: can tolerate **(n-1)/2** failures (n = total nodes)

### 5.3 kube-scheduler — The Placement Engine
- Watches the API server for **newly created pods with no node assigned**
- Selects the best worker node for each pod based on:
  - Available CPU and memory on each node
  - Node affinity/anti-affinity rules
  - Taints and tolerations
  - Pod priority class
  - Data locality (for volumes)
- Two phases: **Filter** (which nodes can run this pod?) → **Score** (which is the best fit?)
- Stateless — can be replaced or scaled; uses leader election for HA

### 5.4 kube-controller-manager — The Reconciliation Loop
- Runs **multiple controllers** in a single binary process
- Each controller watches a resource type and runs a **control loop**:
  ```
  Watch desired state (etcd via apiserver)
       ↓
  Compare to actual state
       ↓
  Take action to reconcile (make actual = desired)
       ↓
  Repeat forever
  ```
- Key controllers included:
  - **Deployment Controller** — manages rollouts and rollbacks
  - **ReplicaSet Controller** — ensures the right number of pods are running
  - **Node Controller** — detects and responds to node failures
  - **Service Account Controller** — creates default accounts for new namespaces
  - **Endpoints Controller** — links Services to the correct Pods

### 5.5 cloud-controller-manager (Optional)
- Connects Kubernetes to cloud provider APIs
- Provisions **LoadBalancer** services as real cloud load balancers (AWS ELB, GCP Load Balancer)
- Attaches/detaches cloud volumes (AWS EBS, Azure Disk)
- Registers/deregisters nodes with cloud compute APIs
- Only present in cloud-managed clusters (EKS, GKE, AKS); self-hosted clusters may not have it

---

## 6. Worker Node Components

### 6.1 kubelet — The Node Agent
- Runs on **every worker node** — the link between the control plane and the node
- Polls the API server for pod specs assigned to its node
- Tells the **container runtime** to pull images and start/stop containers
- Monitors pod health and reports status back to the API server
- Runs **cAdvisor** internally to collect resource metrics (CPU, memory per container)
- Also manages **static pods** (pods defined in `/etc/kubernetes/manifests/`)
- If kubelet dies → node becomes unreachable → controller manager reschedules pods elsewhere

### 6.2 kube-proxy — The Network Rules Manager
- Runs on every node and maintains **network rules** (iptables, IPVS, or eBPF)
- Implements **Kubernetes Services** — routes traffic to the correct pod
- Handles load balancing across all pods that back a Service
- In modern clusters with advanced CNI plugins (Cilium, Calico), kube-proxy is often replaced

### 6.3 Container Runtime — The Engine
- The actual software that **pulls images and runs containers**
- Kubernetes talks to the runtime via the **Container Runtime Interface (CRI)**
- **containerd** is the default runtime since Kubernetes 1.24 (Docker was deprecated as a runtime)
- Alternatives: CRI-O, Docker (via cri-dockerd)

---

## 7. Core Kubernetes Objects

| Object | What It Does |
|--------|-------------|
| **Pod** | Smallest deployable unit; wraps one or more containers |
| **ReplicaSet** | Ensures N copies of a pod are always running |
| **Deployment** | Manages ReplicaSets; handles rolling updates and rollbacks |
| **Service** | Stable network endpoint to access a set of pods |
| **Ingress** | HTTP/HTTPS routing from outside the cluster to Services |
| **ConfigMap** | Stores non-sensitive configuration data (env vars, config files) |
| **Secret** | Stores sensitive data (passwords, tokens) encoded in base64 |
| **Namespace** | Logical isolation within a cluster (like separate environments) |
| **PersistentVolume (PV)** | A piece of storage provisioned in the cluster |
| **PersistentVolumeClaim (PVC)** | A request for storage by a pod |
| **StatefulSet** | Like Deployment but for stateful apps (stable identity per pod) |
| **DaemonSet** | Ensures one pod runs on every node (e.g., logging agents) |
| **Job** | Runs a task to completion (batch processing) |
| **CronJob** | Runs a Job on a schedule (like cron) |
| **HPA** | HorizontalPodAutoscaler — auto-scales pods based on CPU/memory |

---

## 8. Pod — In Depth

A Pod is the **smallest deployable unit** in Kubernetes. It wraps one or more containers that share:
- The same **network namespace** (same IP, same ports)
- The same **storage volumes**
- The same **lifecycle** (start and stop together)

### Pod YAML — Basic
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-app
  labels:
    app: my-app
    environment: production
spec:
  containers:
    - name: app-container
      image: nginx:1.25
      ports:
        - containerPort: 80
      resources:
        requests:
          memory: "64Mi"
          cpu: "250m"
        limits:
          memory: "128Mi"
          cpu: "500m"
      env:
        - name: APP_ENV
          value: "production"
      volumeMounts:
        - name: config-vol
          mountPath: /etc/config
  volumes:
    - name: config-vol
      configMap:
        name: my-config
  restartPolicy: Always   # Always | OnFailure | Never
```

### Multi-Container Pod (Sidecar Pattern)
```yaml
spec:
  containers:
    - name: main-app
      image: myapp:v1.0
      ports:
        - containerPort: 8080
    - name: log-shipper          # sidecar — ships logs to central system
      image: fluentd:latest
      volumeMounts:
        - name: logs
          mountPath: /var/log/app
  volumes:
    - name: logs
      emptyDir: {}               # shared between both containers
```

### Common Container Patterns
| Pattern | Description | Example |
|---------|-------------|---------|
| **Sidecar** | Helper container alongside main app | Log shipper, metrics exporter |
| **Init Container** | Runs BEFORE main container starts | DB migration, config download |
| **Ambassador** | Proxy that handles networking | Local proxy to external service |
| **Adapter** | Standardizes output from main app | Format logs before shipping |

### Init Containers
```yaml
spec:
  initContainers:
    - name: wait-for-db
      image: busybox
      command: ['sh', '-c', 'until nc -z db-service 5432; do sleep 2; done']
  containers:
    - name: app
      image: myapp:v1.0
```
Init containers run **sequentially** and **must succeed** before main containers start.

### Pod Lifecycle
```
Pending → ContainerCreating → Running → Succeeded / Failed
                                  ↑
                            (restart on failure)
```

| Phase | Meaning |
|-------|---------|
| Pending | Pod accepted but containers not started yet (waiting for scheduling or image pull) |
| Running | All containers started; at least one is running |
| Succeeded | All containers completed successfully (exit 0) |
| Failed | All containers exited; at least one failed (non-zero exit) |
| Unknown | Cannot determine pod state (node unreachable) |

---

## 9. Deployments & ReplicaSets

### ReplicaSet
Ensures a **specified number of identical pod replicas** are always running.

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: my-rs
spec:
  replicas: 3                  # always keep 3 pods running
  selector:
    matchLabels:
      app: my-app
  template:                    # pod template
    metadata:
      labels:
        app: my-app
    spec:
      containers:
        - name: app
          image: nginx:1.25
```

If a pod crashes → ReplicaSet immediately creates a replacement.
If you delete a pod manually → ReplicaSet replaces it automatically.

### Deployment (Recommended over ReplicaSet directly)
A **Deployment** manages one or more ReplicaSets and adds:
- Rolling updates (zero downtime deployments)
- Rollback to previous versions
- Declarative updates to pods

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment
  labels:
    app: my-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1           # max pods above desired count during update
      maxUnavailable: 1     # max pods unavailable during update
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
        - name: app
          image: myapp:v2.0
          ports:
            - containerPort: 8080
          resources:
            requests:
              memory: "128Mi"
              cpu: "250m"
            limits:
              memory: "256Mi"
              cpu: "500m"
```

### Deployment vs ReplicaSet

| Feature | ReplicaSet | Deployment |
|---------|-----------|------------|
| Keeps N replicas running | ✅ | ✅ |
| Rolling updates | ❌ | ✅ |
| Rollback | ❌ | ✅ |
| Update history | ❌ | ✅ |
| Use directly in production | ⚠️ Rarely | ✅ Always |

---

## 10. Services & Networking

A **Service** gives a stable IP and DNS name to a dynamic set of pods (pods are ephemeral — they change IPs when restarted). Services use **label selectors** to find which pods they route to.

### Service Types

#### 1. ClusterIP (Default) — Internal Access Only
```yaml
apiVersion: v1
kind: Service
metadata:
  name: backend-service
spec:
  type: ClusterIP              # only accessible inside the cluster
  selector:
    app: backend
  ports:
    - port: 80                 # service port (what clients use)
      targetPort: 8080         # container port (where app listens)
```
```
Inside Cluster:  frontend → backend-service:80 → Pod A (8080)
                                              → Pod B (8080)   (load balanced)
                                              → Pod C (8080)
Outside Cluster: ❌ Not reachable
```

#### 2. NodePort — External Access via Node IP
```yaml
spec:
  type: NodePort
  selector:
    app: backend
  ports:
    - port: 80                 # internal cluster port
      targetPort: 8080         # container port
      nodePort: 30080          # external port (30000–32767 range)
```
```
Outside:  http://NodeIP:30080 → Service → Pods
```

#### 3. LoadBalancer — Cloud Load Balancer
```yaml
spec:
  type: LoadBalancer
  selector:
    app: backend
  ports:
    - port: 80
      targetPort: 8080
```
```
Cloud provision:  AWS ELB / GCP LB / Azure LB
Outside:  http://EXTERNAL_IP:80 → LoadBalancer → Service → Pods
```
Only works on cloud providers (EKS, GKE, AKS). On bare metal, use MetalLB.

#### 4. ExternalName — DNS Alias
```yaml
spec:
  type: ExternalName
  externalName: db.mycompany.com   # maps Service to an external DNS name
```
Used to route traffic inside the cluster to an external service by name.

### Service Summary Table

| Type | Accessible From | Use Case |
|------|----------------|----------|
| ClusterIP | Inside cluster only | Microservice-to-microservice communication |
| NodePort | Outside via NodeIP:Port | Dev/testing, simple external access |
| LoadBalancer | Outside via cloud LB IP | Production external traffic on cloud |
| ExternalName | Inside cluster | Map internal name to external DNS |

### DNS in Kubernetes
Every Service gets a DNS name automatically:
```
service-name.namespace.svc.cluster.local

Examples:
backend-service.default.svc.cluster.local
postgres.database.svc.cluster.local
```
Pods in the same namespace can use just the service name: `backend-service`

---

## 11. Ingress

An **Ingress** manages **HTTP/HTTPS routing** from outside the cluster to Services. It acts as a reverse proxy / API gateway at the cluster edge.

### Why Ingress?
- LoadBalancer Service → one external IP per service → expensive
- Ingress → **one external IP**, route to many services by path or hostname

```
Internet
    ↓
[ Ingress Controller (nginx/traefik) ]
    ↓
/api      → api-service
/frontend → frontend-service
/admin    → admin-service
```

### Ingress YAML Example
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  ingressClassName: nginx
  rules:
    - host: myapp.example.com
      http:
        paths:
          - path: /api
            pathType: Prefix
            backend:
              service:
                name: api-service
                port:
                  number: 80
          - path: /
            pathType: Prefix
            backend:
              service:
                name: frontend-service
                port:
                  number: 80
  tls:
    - hosts:
        - myapp.example.com
      secretName: tls-secret     # TLS certificate stored as a Secret
```

### Popular Ingress Controllers
| Controller | Best For |
|-----------|---------|
| NGINX Ingress | General purpose, most popular |
| Traefik | Auto-discovery, great with Docker Compose too |
| AWS ALB Controller | AWS-native, integrates with ALB |
| Kong | API Gateway features, rate limiting, auth |

---

## 12. ConfigMap & Secrets

### ConfigMap — Non-Sensitive Configuration
Stores key-value configuration data separately from application code.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  DATABASE_HOST: "postgres-service"
  DATABASE_PORT: "5432"
  APP_ENV: "production"
  config.json: |
    {
      "maxConnections": 100,
      "timeout": 30
    }
```

**Use ConfigMap as environment variables in Pod:**
```yaml
spec:
  containers:
    - name: app
      image: myapp:v1.0
      envFrom:
        - configMapRef:
            name: app-config         # inject ALL keys as env vars
      env:
        - name: DB_HOST
          valueFrom:
            configMapKeyRef:
              name: app-config
              key: DATABASE_HOST     # inject ONE specific key
```

**Use ConfigMap as a mounted file:**
```yaml
      volumeMounts:
        - name: config-volume
          mountPath: /etc/app/config
  volumes:
    - name: config-volume
      configMap:
        name: app-config
```

### Secret — Sensitive Data
Stores sensitive data encoded in **base64**. Values are NOT encrypted by default (use encryption at rest + RBAC to secure).

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: db-secret
type: Opaque
data:
  DB_PASSWORD: cGFzc3dvcmQxMjM=    # base64 encoded "password123"
  API_KEY: bXlzZWNyZXRrZXk=       # base64 encoded "mysecretkey"
```

```bash
# Encode a value for Secret
echo -n "password123" | base64   # output: cGFzc3dvcmQxMjM=

# Decode a Secret value
echo "cGFzc3dvcmQxMjM=" | base64 --decode
```

**Use Secret in Pod:**
```yaml
      envFrom:
        - secretRef:
            name: db-secret
```

### ConfigMap vs Secret

| Feature | ConfigMap | Secret |
|---------|-----------|--------|
| Data type | Plain text | Base64 encoded |
| Sensitive data | ❌ No | ✅ Yes |
| Encrypted at rest | ❌ (default) | ✅ (with encryption config) |
| Use case | App config, feature flags | Passwords, tokens, certs |

---

## 13. Volumes & Persistent Storage

Containers are ephemeral — data in their filesystem is lost when the container restarts. Volumes solve this.

### emptyDir — Temporary Shared Storage
```yaml
volumes:
  - name: shared-data
    emptyDir: {}            # created when pod starts, deleted when pod dies
```
Use case: Sharing files between containers in the same pod.

### hostPath — Mount Host Directory
```yaml
volumes:
  - name: host-logs
    hostPath:
      path: /var/log/app
      type: Directory
```
Use case: Accessing node-level resources (logs, devices). Avoid in production — ties pod to specific node.

### PersistentVolume (PV) + PersistentVolumeClaim (PVC)

**PersistentVolume (PV)** — a piece of storage provisioned by an administrator or dynamically:
```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: my-pv
spec:
  capacity:
    storage: 10Gi
  accessModes:
    - ReadWriteOnce          # RWO: one node read/write
    # ReadOnlyMany (ROX): many nodes read-only
    # ReadWriteMany (RWX): many nodes read/write
  persistentVolumeReclaimPolicy: Retain   # Retain | Delete | Recycle
  storageClassName: standard
  hostPath:
    path: /data/my-pv       # for local dev; use cloud storage in prod
```

**PersistentVolumeClaim (PVC)** — a request for storage by a pod:
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 5Gi
  storageClassName: standard
```

**Use PVC in a Pod:**
```yaml
spec:
  containers:
    - name: db
      image: postgres:15
      volumeMounts:
        - name: db-storage
          mountPath: /var/lib/postgresql/data
  volumes:
    - name: db-storage
      persistentVolumeClaim:
        claimName: my-pvc
```

### Dynamic Provisioning with StorageClass
Instead of manually creating PVs, StorageClass auto-provisions storage when a PVC is created:
```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: fast-ssd
provisioner: kubernetes.io/aws-ebs    # or gce-pd, azure-disk, etc.
parameters:
  type: gp3
  fsType: ext4
reclaimPolicy: Delete
```

---

## 14. Namespaces

A **Namespace** provides logical isolation within a single cluster — like separate virtual clusters.

```bash
# Default namespaces
kubectl get namespaces

NAME              STATUS
default           Active    # where objects go if no namespace specified
kube-system       Active    # Kubernetes system components (apiserver, etc.)
kube-public       Active    # publicly readable, rarely used
kube-node-lease   Active    # node heartbeat objects
```

### Creating and Using Namespaces
```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: production

---
apiVersion: v1
kind: Namespace
metadata:
  name: staging
```

```bash
# Create namespace
kubectl create namespace development

# Deploy to a specific namespace
kubectl apply -f deployment.yaml -n production

# List pods in a namespace
kubectl get pods -n production

# List pods across ALL namespaces
kubectl get pods --all-namespaces
kubectl get pods -A
```

### Resource Quota per Namespace
```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: dev-quota
  namespace: development
spec:
  hard:
    pods: "20"
    requests.cpu: "4"
    requests.memory: 8Gi
    limits.cpu: "8"
    limits.memory: 16Gi
```

---

## 15. Scheduling — Taints, Tolerations, Affinity

### Labels & Selectors
Labels are key-value pairs attached to objects. Selectors filter objects by labels.
```yaml
metadata:
  labels:
    app: backend
    tier: api
    environment: production
```

### Node Selector — Simple Node Targeting
```yaml
spec:
  nodeSelector:
    disktype: ssd          # only schedule on nodes labeled disktype=ssd
```
```bash
kubectl label node node1 disktype=ssd
```

### Taints & Tolerations
**Taints** are placed on Nodes — they **repel** pods from being scheduled there unless the pod has a matching **Toleration**.

```bash
# Add a taint to a node
kubectl taint nodes node1 gpu=true:NoSchedule
# Effect options: NoSchedule | PreferNoSchedule | NoExecute
```

```yaml
# Pod tolerates the taint → can be scheduled on that node
spec:
  tolerations:
    - key: "gpu"
      operator: "Equal"
      value: "true"
      effect: "NoSchedule"
```

**Use case:** Taint GPU nodes → only GPU workloads (with tolerations) schedule there.

### Node Affinity — Flexible Node Rules
```yaml
spec:
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:   # HARD rule
        nodeSelectorTerms:
          - matchExpressions:
              - key: zone
                operator: In
                values: [us-east-1a, us-east-1b]
      preferredDuringSchedulingIgnoredDuringExecution:  # SOFT rule
        - weight: 1
          preference:
            matchExpressions:
              - key: disktype
                operator: In
                values: [ssd]
```

### Pod Affinity / Anti-Affinity
```yaml
spec:
  affinity:
    podAntiAffinity:    # spread pods across different nodes
      requiredDuringSchedulingIgnoredDuringExecution:
        - labelSelector:
            matchExpressions:
              - key: app
                operator: In
                values: [backend]
          topologyKey: "kubernetes.io/hostname"
```
**Use case:** Ensure no two backend replicas land on the same node — high availability.

---

## 16. Deployment Strategies

### 1. RollingUpdate (Default) — Zero Downtime
```yaml
strategy:
  type: RollingUpdate
  rollingUpdate:
    maxSurge: 1           # max extra pods during update
    maxUnavailable: 0     # never reduce below desired count
```
```
Old: [v1][v1][v1]
          ↓
Step 1: [v1][v1][v1][v2]   (surge: +1)
Step 2: [v1][v1][v2]       (one v1 removed)
Step 3: [v1][v2][v2]
Step 4: [v2][v2][v2]       ✅ complete
```

### 2. Recreate — Downtime Between Versions
```yaml
strategy:
  type: Recreate
```
All old pods are killed first, then new pods are started. There is a brief downtime period.
Use case: When old and new versions cannot run simultaneously (e.g., DB schema incompatibility).

### 3. Blue/Green Deployment — Instant Cutover
```
Blue (v1 — live) ← Service → (v2 — standby) Green

When ready: update Service selector to point to Green
```
- Two full deployments run simultaneously
- Traffic switch is instantaneous and reversible
- Requires double the resources during transition

### 4. Canary Deployment — Gradual Rollout
```
100% users → v1
 10% users → v2 (canary — watch for errors)

If stable after monitoring:
 50% users → v2
100% users → v2
```
- Gradual traffic shift, easy to catch problems early
- Can be done with weighted Services or Ingress rules

---

## 17. Probes — Liveness, Readiness, Startup

Probes tell Kubernetes whether a container is healthy and ready to receive traffic.

### Liveness Probe — Is the container alive?
```yaml
livenessProbe:
  httpGet:
    path: /healthz
    port: 8080
  initialDelaySeconds: 30    # wait 30s before first check
  periodSeconds: 10          # check every 10s
  failureThreshold: 3        # restart after 3 failures
```
If the liveness probe fails → Kubernetes **restarts the container**.

### Readiness Probe — Is the container ready for traffic?
```yaml
readinessProbe:
  httpGet:
    path: /ready
    port: 8080
  initialDelaySeconds: 10
  periodSeconds: 5
  failureThreshold: 3
```
If the readiness probe fails → Kubernetes **removes the pod from Service endpoints** (stops sending traffic) but does NOT restart it.

### Startup Probe — For slow-starting containers
```yaml
startupProbe:
  httpGet:
    path: /healthz
    port: 8080
  failureThreshold: 30       # give up to 5 minutes to start (30 * 10s)
  periodSeconds: 10
```
If the startup probe is defined, liveness and readiness probes are disabled until it succeeds.
Use case: Apps that take a long time to initialize (e.g., loading a large ML model).

### Probe Types

| Probe | What happens on failure |
|-------|------------------------|
| **Liveness** | Container is restarted |
| **Readiness** | Pod removed from Service load balancer; NOT restarted |
| **Startup** | Container restarted if it doesn't start in time |

### Probe Mechanisms

| Type | Example | Use Case |
|------|---------|---------|
| `httpGet` | GET /health → 200 OK | HTTP apps |
| `tcpSocket` | Can connect to port? | TCP services, databases |
| `exec` | Run command; 0 exit = healthy | Custom health logic |
| `grpc` | gRPC health check | gRPC services |

---

## 18. StatefulSets & DaemonSets

### StatefulSet — For Stateful Applications
Manages pods that need:
- **Stable, unique pod names** (pod-0, pod-1, pod-2 — never random)
- **Stable persistent storage** (each pod gets its own PVC)
- **Ordered startup and shutdown** (pod-0 starts first, pod-2 deletes first)

```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: postgres
spec:
  serviceName: "postgres"       # must have a Headless Service
  replicas: 3
  selector:
    matchLabels:
      app: postgres
  template:
    metadata:
      labels:
        app: postgres
    spec:
      containers:
        - name: postgres
          image: postgres:15
          volumeMounts:
            - name: data
              mountPath: /var/lib/postgresql/data
  volumeClaimTemplates:          # each pod gets its own PVC
    - metadata:
        name: data
      spec:
        accessModes: [ReadWriteOnce]
        resources:
          requests:
            storage: 10Gi
```

**Use cases:** Databases (PostgreSQL, MySQL, MongoDB, Cassandra), Kafka brokers, Zookeeper.

### Deployment vs StatefulSet

| Feature | Deployment | StatefulSet |
|---------|-----------|------------|
| Pod names | Random (my-app-xyz) | Stable (my-app-0, my-app-1) |
| Storage | Shared or none | Each pod gets its own PVC |
| Startup order | Parallel (any order) | Sequential (0, 1, 2...) |
| Delete order | Any order | Reverse sequential (2, 1, 0) |
| Use case | Stateless apps | Stateful apps (databases, queues) |

### DaemonSet — One Pod Per Node
Ensures exactly **one pod runs on every node** (or every node matching a selector).

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: log-collector
spec:
  selector:
    matchLabels:
      app: log-collector
  template:
    metadata:
      labels:
        app: log-collector
    spec:
      containers:
        - name: fluentd
          image: fluentd:v1.16
          volumeMounts:
            - name: varlog
              mountPath: /var/log
      volumes:
        - name: varlog
          hostPath:
            path: /var/log
```

**Use cases:** Log collectors (Fluentd, Filebeat), monitoring agents (Prometheus node_exporter), network plugins (Calico, Cilium), security agents.

### Job & CronJob

**Job** — runs a task to completion:
```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: db-migration
spec:
  completions: 1
  parallelism: 1
  template:
    spec:
      containers:
        - name: migrator
          image: myapp:migrate
          command: ["python", "manage.py", "migrate"]
      restartPolicy: OnFailure
```

**CronJob** — runs a Job on a schedule:
```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: daily-report
spec:
  schedule: "0 9 * * *"          # every day at 9 AM (cron syntax)
  jobTemplate:
    spec:
      template:
        spec:
          containers:
            - name: reporter
              image: myapp:reporter
              command: ["python", "send_report.py"]
          restartPolicy: OnFailure
```

---

## 19. Resource Quotas & Limits

### Resource Requests vs Limits

```yaml
resources:
  requests:
    memory: "128Mi"    # minimum guaranteed; used for scheduling decisions
    cpu: "250m"        # 250 millicores = 0.25 CPU core
  limits:
    memory: "256Mi"    # maximum allowed; container killed if exceeded
    cpu: "500m"        # throttled if exceeded (not killed)
```

**CPU units:**
- `1` = 1 full CPU core
- `500m` = 0.5 CPU core (500 millicores)
- `250m` = 0.25 CPU core

**Memory units:**
- `128Mi` = 128 Mebibytes
- `1Gi` = 1 Gibibyte

### HorizontalPodAutoscaler (HPA)
Automatically scales pods based on CPU/memory usage.

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: backend-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: backend
  minReplicas: 2
  maxReplicas: 10
  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 70    # scale up if avg CPU > 70%
```

---

## 20. RBAC — Role-Based Access Control

RBAC controls **who can do what** in a Kubernetes cluster.

### Key Objects

| Object | Scope | What it does |
|--------|-------|-------------|
| **Role** | Namespace | Grants permissions within a namespace |
| **ClusterRole** | Cluster-wide | Grants permissions across the entire cluster |
| **RoleBinding** | Namespace | Binds a Role to a user/group/serviceaccount |
| **ClusterRoleBinding** | Cluster-wide | Binds a ClusterRole to a user cluster-wide |

```yaml
# Role: allow reading pods in "production" namespace
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: production
  name: pod-reader
rules:
  - apiGroups: [""]
    resources: ["pods"]
    verbs: ["get", "list", "watch"]   # read-only

---
# RoleBinding: give user "alice" the pod-reader role
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: read-pods
  namespace: production
subjects:
  - kind: User
    name: alice
    apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io
```

---

## 21. Helm — Kubernetes Package Manager

**Helm** is the package manager for Kubernetes — like `apt` for Ubuntu or `pip` for Python.

- A **Chart** is a Helm package (collection of YAML templates)
- A **Release** is a deployed instance of a chart
- A **Repository** is where charts are stored

```bash
# Add a chart repository
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update

# Search for a chart
helm search repo postgres

# Install a chart (creates a Release)
helm install my-postgres bitnami/postgresql \
  --set auth.postgresPassword=secret \
  --namespace database \
  --create-namespace

# List all releases
helm list -A

# Upgrade a release
helm upgrade my-postgres bitnami/postgresql --set replicas=3

# Rollback a release
helm rollback my-postgres 1       # rollback to revision 1

# Uninstall a release
helm uninstall my-postgres

# Show values for a chart
helm show values bitnami/postgresql

# Install with custom values file
helm install my-postgres bitnami/postgresql -f my-values.yaml
```

---

## 22. All kubectl Commands — Explained

### CLUSTER & CONTEXT

```bash
# View all available contexts (clusters configured in kubeconfig)
kubectl config get-contexts

# Switch to a different cluster context
kubectl config use-context my-cluster

# View current context
kubectl config current-context

# View cluster info (API server address)
kubectl cluster-info

# Check API server and component health
kubectl get componentstatuses

# List all nodes in the cluster
kubectl get nodes

# Detailed info about a node (resources, pods running, conditions)
kubectl describe node node1

# View node resource usage (requires metrics-server)
kubectl top nodes
```

---

### PODS

```bash
# List all pods in current namespace
kubectl get pods

# List pods in a specific namespace
kubectl get pods -n production

# List pods across ALL namespaces
kubectl get pods -A
kubectl get pods --all-namespaces

# List pods with more info (node, IP, age)
kubectl get pods -o wide

# Watch pods in real time (updates like top command)
kubectl get pods -w

# Get detailed description of a pod (events, conditions, mounts)
kubectl describe pod my-pod

# View logs of a pod
kubectl logs my-pod

# View logs of a specific container in a multi-container pod
kubectl logs my-pod -c container-name

# Follow / tail logs in real time
kubectl logs -f my-pod

# Get last 100 lines of logs
kubectl logs my-pod --tail=100

# View logs since a specific time
kubectl logs my-pod --since=1h

# Open an interactive shell inside a pod
kubectl exec -it my-pod -- bash
kubectl exec -it my-pod -- sh             # for Alpine-based images

# Execute a single command in a pod
kubectl exec my-pod -- ls /app
kubectl exec my-pod -- python manage.py migrate

# Copy files between pod and local machine
kubectl cp my-pod:/app/output.txt ./output.txt    # pod → local
kubectl cp ./config.py my-pod:/app/config.py       # local → pod

# View pod resource usage (requires metrics-server)
kubectl top pods

# Delete a pod (Deployment will recreate it)
kubectl delete pod my-pod

# Force delete a stuck pod
kubectl delete pod my-pod --grace-period=0 --force

# Run a one-off interactive pod for debugging
kubectl run debug-pod --image=busybox --rm -it --restart=Never -- sh
```

---

### DEPLOYMENTS

```bash
# List all deployments
kubectl get deployments
kubectl get deploy

# Describe a deployment (events, strategy, conditions)
kubectl describe deployment my-deployment

# Create a deployment from YAML
kubectl apply -f deployment.yaml

# Create a quick deployment from command line
kubectl create deployment my-app --image=nginx:1.25 --replicas=3

# Scale a deployment up or down
kubectl scale deployment my-deployment --replicas=5

# Update image of a container in a deployment (triggers rolling update)
kubectl set image deployment/my-deployment app=myapp:v2.0

# Check rollout status
kubectl rollout status deployment/my-deployment

# View rollout history
kubectl rollout history deployment/my-deployment

# View details of a specific rollout revision
kubectl rollout history deployment/my-deployment --revision=2

# Rollback to previous version
kubectl rollout undo deployment/my-deployment

# Rollback to a specific revision
kubectl rollout undo deployment/my-deployment --to-revision=2

# Pause a rolling update
kubectl rollout pause deployment/my-deployment

# Resume a paused rollout
kubectl rollout resume deployment/my-deployment

# Restart all pods in a deployment (rolling restart)
kubectl rollout restart deployment/my-deployment

# Delete a deployment
kubectl delete deployment my-deployment
```

---

### SERVICES

```bash
# List all services
kubectl get services
kubectl get svc

# Describe a service (endpoints, selector, port mappings)
kubectl describe service my-service

# Create a service from YAML
kubectl apply -f service.yaml

# Expose a deployment as a Service (quick)
kubectl expose deployment my-deployment --port=80 --target-port=8080 --type=ClusterIP

# Expose as NodePort
kubectl expose deployment my-deployment --port=80 --type=NodePort

# Forward a service port to localhost (for local testing)
kubectl port-forward service/my-service 8080:80
# Now access http://localhost:8080 → routes to the service

# Forward a pod port directly
kubectl port-forward pod/my-pod 8080:8080

# Delete a service
kubectl delete service my-service
```

---

### CONFIGMAPS & SECRETS

```bash
# Create a ConfigMap from literal values
kubectl create configmap app-config \
  --from-literal=DB_HOST=postgres \
  --from-literal=DB_PORT=5432

# Create a ConfigMap from a file
kubectl create configmap app-config --from-file=config.json

# Create a ConfigMap from a directory
kubectl create configmap app-config --from-file=./config-dir/

# List all ConfigMaps
kubectl get configmaps
kubectl get cm

# View ConfigMap contents
kubectl describe configmap app-config
kubectl get configmap app-config -o yaml

# Create a Secret from literal values
kubectl create secret generic db-secret \
  --from-literal=DB_PASSWORD=mysecret \
  --from-literal=API_KEY=abc123

# Create a Secret from files
kubectl create secret generic tls-secret \
  --from-file=tls.crt=./server.crt \
  --from-file=tls.key=./server.key

# Create a TLS Secret
kubectl create secret tls my-tls \
  --cert=server.crt \
  --key=server.key

# List Secrets
kubectl get secrets

# Decode a secret value
kubectl get secret db-secret -o jsonpath='{.data.DB_PASSWORD}' | base64 --decode

# Delete a ConfigMap or Secret
kubectl delete configmap app-config
kubectl delete secret db-secret
```

---

### NAMESPACES

```bash
# List all namespaces
kubectl get namespaces
kubectl get ns

# Create a namespace
kubectl create namespace production
kubectl apply -f namespace.yaml

# Set default namespace for current session
kubectl config set-context --current --namespace=production

# List all resources in a namespace
kubectl get all -n production

# Delete a namespace (deletes EVERYTHING inside it)
kubectl delete namespace development
```

---

### NODES

```bash
# List all nodes
kubectl get nodes

# Detailed node information
kubectl describe node node1

# Mark a node as unschedulable (stops new pods being placed here)
kubectl cordon node1

# Remove cordon (allow scheduling again)
kubectl uncordon node1

# Drain a node (evict all pods, for maintenance)
kubectl drain node1 --ignore-daemonsets --delete-emptydir-data

# Add a label to a node
kubectl label node node1 disktype=ssd

# Add a taint to a node
kubectl taint nodes node1 gpu=true:NoSchedule

# Remove a taint from a node
kubectl taint nodes node1 gpu=true:NoSchedule-
```

---

### RESOURCE MANAGEMENT

```bash
# Apply YAML file (create or update — idempotent)
kubectl apply -f resource.yaml

# Apply all YAML files in a directory
kubectl apply -f ./k8s/

# Apply from a URL
kubectl apply -f https://raw.githubusercontent.com/example/app.yaml

# Create resource (fails if already exists)
kubectl create -f resource.yaml

# Delete resource from YAML file
kubectl delete -f resource.yaml

# Delete a specific resource
kubectl delete pod my-pod
kubectl delete deployment my-deployment
kubectl delete service my-service

# Get resource in different output formats
kubectl get pod my-pod -o yaml    # full YAML definition
kubectl get pod my-pod -o json    # full JSON definition
kubectl get pod my-pod -o wide    # extra columns (node, IP)

# Edit a resource in-place (opens editor)
kubectl edit deployment my-deployment

# Patch a resource field
kubectl patch deployment my-deployment -p '{"spec":{"replicas":5}}'

# List ALL resource types in current namespace
kubectl get all

# Explain a resource field (documentation in terminal)
kubectl explain pod.spec.containers
kubectl explain deployment.spec.strategy

# Check if you have permission to do something
kubectl auth can-i create pods --namespace production
kubectl auth can-i delete deployments

# View all API resources available in the cluster
kubectl api-resources

# Check cluster events (great for debugging)
kubectl get events --sort-by='.lastTimestamp'
kubectl get events -n production
```

---

### INGRESS & STORAGE

```bash
# List Ingress resources
kubectl get ingress
kubectl get ing

# Describe an Ingress
kubectl describe ingress my-ingress

# List PersistentVolumes
kubectl get pv

# List PersistentVolumeClaims
kubectl get pvc
kubectl get pvc -n production

# Describe a PVC
kubectl describe pvc my-pvc

# Delete a PVC (WARNING: may delete data depending on reclaim policy)
kubectl delete pvc my-pvc
```

---

## 23. Best Practices

### ✅ DOs

**Resources:**
- Always set **resource requests and limits** on every container — prevents one pod from starving others
- Use **HPA** (HorizontalPodAutoscaler) to auto-scale based on real load
- Use **namespaces** to isolate environments (dev, staging, production)
- Apply **ResourceQuota** per namespace to enforce limits

**Reliability:**
- Always configure **liveness and readiness probes** — ensures healthy routing and auto-recovery
- Use **Deployments** (not bare Pods or ReplicaSets directly)
- Use **RollingUpdate** strategy with `maxUnavailable: 0` for zero-downtime deployments
- Use **PodDisruptionBudgets** to ensure minimum availability during node drains

**Security:**
- Enable **RBAC** — principle of least privilege for all users and service accounts
- Never run containers as **root** — set `securityContext.runAsNonRoot: true`
- Use **Secrets** for sensitive data (not ConfigMaps); enable encryption at rest
- Set `readOnlyRootFilesystem: true` in securityContext where possible
- Always scan images before deploying with tools like Trivy or Snyk

**Storage:**
- Use **PersistentVolumes** for any stateful data — never rely on container filesystem
- Use **StatefulSets** for databases and message queues — not Deployments

**Networking:**
- Use **NetworkPolicies** to restrict pod-to-pod communication (default: all pods can talk to all)
- Use **Ingress** to consolidate external access (not one LoadBalancer per service)

### ❌ DON'Ts

- Never deploy to `default` namespace in production — use named namespaces
- Never use `latest` image tag in production — use pinned versions
- Never skip resource requests/limits — it leads to noisy-neighbor problems
- Never put secrets in ConfigMaps or environment variable literals in YAML
- Never manually delete pods managed by a Deployment — the controller recreates them; fix the root cause
- Never ignore pod events — `kubectl describe pod` events tell you why a pod is failing

---

## 24. Kubernetes vs Docker Swarm

| Feature | Kubernetes | Docker Swarm |
|---------|-----------|-------------|
| Complexity | High (steep learning curve) | Low (easy to set up) |
| Scalability | Massive (production-grade) | Moderate |
| Auto-scaling | ✅ HPA, VPA | ❌ Manual only |
| Self-healing | ✅ Advanced | ✅ Basic |
| Rolling updates | ✅ Fine-grained control | ✅ Basic |
| Load balancing | ✅ Built-in + Ingress | ✅ Basic |
| Storage | ✅ PV, PVC, StorageClass | ✅ Limited |
| RBAC | ✅ Full | ❌ Basic |
| Ecosystem | ✅ Massive (Helm, Istio, etc.) | ❌ Minimal |
| Cloud support | ✅ EKS, GKE, AKS, etc. | ❌ Limited |
| Best For | Large, production workloads | Small teams, simple apps |

> **Verdict:** Kubernetes is the industry standard for production. Docker Swarm is fine for small-scale simple deployments but cannot match Kubernetes at scale.

---

## 25. Common Interview Questions

### Q1: What is Kubernetes and why is it needed?
**Answer:** Kubernetes is an open-source container orchestration platform that automates deployment, scaling, and management of containerized applications. It is needed because when applications scale to hundreds or thousands of containers, manually managing them becomes impossible — you need automated scheduling, self-healing, rolling updates, load balancing, and service discovery. Docker alone cannot provide these at scale.

### Q2: Explain the Kubernetes architecture.
**Answer:** Kubernetes has two layers. The **Control Plane** (brain) contains: kube-apiserver (central hub for all requests), etcd (distributed key-value store — the single source of truth), kube-scheduler (places pods on the best node), and kube-controller-manager (runs control loops to reconcile desired vs actual state). The **Worker Nodes** (muscle) contain: kubelet (node agent that runs pods), kube-proxy (manages network rules), and a container runtime (containerd — pulls images and runs containers).

### Q3: What is a Pod? Can a Pod have multiple containers?
**Answer:** A Pod is the smallest deployable unit in Kubernetes. It wraps one or more containers that share the same network namespace (same IP address), storage volumes, and lifecycle. Yes — multi-container pods are common using patterns like sidecar (helper container, e.g., log shipper), init containers (setup tasks before main app starts), and ambassador (local proxy).

### Q4: What is the difference between a Deployment and a StatefulSet?
**Answer:** A **Deployment** is for stateless applications — pods have random names, share storage or have none, and start in parallel. A **StatefulSet** is for stateful applications — pods have stable ordered names (pod-0, pod-1), each pod gets its own PersistentVolumeClaim, and pods start/stop sequentially. Use StatefulSets for databases (PostgreSQL, MySQL), message brokers (Kafka, Zookeeper), and any app needing stable identity.

### Q5: What is the difference between ClusterIP, NodePort, and LoadBalancer services?
**Answer:** **ClusterIP** (default) is only accessible inside the cluster — for microservice-to-microservice communication. **NodePort** exposes the service on a static port (30000–32767) on every node's IP — accessible from outside but not production-ready. **LoadBalancer** provisions a cloud load balancer with a public IP — the production choice for external traffic on cloud platforms like AWS, GCP, Azure.

### Q6: What is etcd and why is it important?
**Answer:** etcd is a distributed, strongly-consistent key-value store that is the single source of truth for the entire cluster state — all objects (pods, deployments, secrets, configs), desired state, and node information. It is the only component that kube-apiserver directly reads and writes. If etcd is lost and not backed up, the entire cluster state is gone. Always back up etcd in production.

### Q7: What is the difference between a liveness probe and a readiness probe?
**Answer:** A **liveness probe** checks if a container is still alive. If it fails, Kubernetes restarts the container. A **readiness probe** checks if a container is ready to receive traffic. If it fails, Kubernetes removes the pod from the Service's endpoints (stops sending traffic) but does NOT restart it. Liveness = is it dead? Readiness = is it prepared to serve?

### Q8: What is a ConfigMap and when would you use it instead of a Secret?
**Answer:** A ConfigMap stores non-sensitive configuration data (database hostnames, feature flags, config file contents) as key-value pairs. A Secret stores sensitive data (passwords, API keys, certificates) encoded in base64. Use ConfigMap for any config that is safe to expose; use Secret for anything that must be kept confidential. Secrets can optionally be encrypted at rest with Kubernetes encryption configuration.

### Q9: What happens when you run `kubectl apply -f deployment.yaml`?
**Answer:** (1) kubectl sends an HTTPS request to kube-apiserver with the YAML content. (2) apiserver authenticates and authorizes the request. (3) apiserver validates the YAML and stores the desired state in etcd. (4) The Deployment Controller (in controller-manager) detects the new/updated Deployment and creates/updates a ReplicaSet. (5) The ReplicaSet controller sees unscheduled pods. (6) kube-scheduler assigns each pod to the best available node and updates etcd. (7) kubelet on the assigned node picks up the pod spec, tells containerd to pull the image and start the container. (8) kubelet reports pod status back to apiserver.

### Q10: What is a DaemonSet and when do you use it?
**Answer:** A DaemonSet ensures exactly one pod runs on every node in the cluster (or on nodes matching a selector). When a new node joins the cluster, the DaemonSet automatically places a pod on it. Use cases: log collectors (Fluentd, Filebeat), monitoring agents (Prometheus node exporter), network plugins (Calico, Cilium), and security agents — anything that needs to run on every node to observe or manage that node.

---

## 🔄 Kubernetes — Complete Flow at a Glance

```
Developer writes app → Dockerizes it → Pushes image to registry
        ↓
Writes Kubernetes YAML (Deployment + Service + Ingress + ConfigMap + Secret)
        ↓
kubectl apply -f ./k8s/
        ↓
kube-apiserver validates → stores in etcd
        ↓
kube-scheduler → picks best node for each pod
        ↓
kubelet on node → pulls image → starts container (via containerd)
        ↓
kube-proxy → sets up network rules for Service routing
        ↓
Ingress Controller → routes external HTTP/HTTPS to correct Service
        ↓
App is live, self-healing, scalable, load-balanced ✅
```

---

## 📝 Quick Revision Card

| Concept | In One Line |
|---------|-------------|
| Kubernetes (K8s) | Open-source container orchestration; automates deploy, scale, manage |
| Control Plane | Brain of cluster: apiserver, etcd, scheduler, controller-manager |
| Worker Node | Muscle of cluster: kubelet, kube-proxy, container runtime |
| kube-apiserver | Central hub; only component that talks to etcd; all requests flow through it |
| etcd | Distributed key-value store; single source of truth for entire cluster state |
| kube-scheduler | Decides which node each pod runs on |
| controller-manager | Runs control loops; reconciles desired vs actual state; self-healing |
| kubelet | Node agent; ensures pods run as expected; reports to apiserver |
| kube-proxy | Manages iptables/IPVS network rules to implement Services |
| Pod | Smallest unit; one or more containers sharing network + storage |
| ReplicaSet | Ensures N identical pod replicas are always running |
| Deployment | Manages ReplicaSets; adds rolling updates and rollback capability |
| StatefulSet | Like Deployment but for stateful apps; stable names, own PVC per pod |
| DaemonSet | One pod per node; for agents, monitoring, networking |
| Job | Runs a task to completion; for batch processing |
| CronJob | Runs a Job on a cron schedule |
| Service | Stable network endpoint; routes traffic to matching pods |
| ClusterIP | Internal-only Service |
| NodePort | External access via NodeIP:Port |
| LoadBalancer | External access via cloud load balancer |
| Ingress | HTTP/HTTPS routing; one external IP to many services |
| ConfigMap | Non-sensitive config data; injected as env vars or files |
| Secret | Sensitive data (base64 encoded); passwords, tokens, certs |
| PersistentVolume | Storage resource in the cluster |
| PVC | Request for storage by a pod |
| StorageClass | Dynamic storage provisioning (auto-create PVs) |
| Namespace | Logical isolation within cluster (dev/staging/prod) |
| Taint | Repels pods from a node unless they have a matching toleration |
| Toleration | Allows a pod to be scheduled on a tainted node |
| Node Affinity | Rules for which nodes a pod prefers or requires |
| Liveness Probe | Restarts container if check fails |
| Readiness Probe | Removes pod from Service if check fails (no restart) |
| Startup Probe | Disables liveness/readiness until app finishes starting |
| HPA | Auto-scales pod count based on CPU/memory metrics |
| RBAC | Role-based access control; who can do what in the cluster |
| Helm | Kubernetes package manager; install complex apps with one command |
| kubectl | CLI tool to interact with Kubernetes |

---

> 💡 **Production Golden Rule for Kubernetes:**
> Always combine — **Resource Requests+Limits + Liveness+Readiness Probes + Rolling Update Strategy + RBAC + Namespaces + Secrets Encryption + Network Policies + HPA**.
> These form the foundation of a reliable, secure, production-grade Kubernetes cluster.

---
*Notes prepared with research from: Kubernetes Official Docs, DevOpsCube (Mar 2026), CloudOptimo (May 2026), DevOps Training Institute (Dec 2025), KodeKloud (Feb 2026), Hero Vired (2025), Portainer Blog (2026)*
