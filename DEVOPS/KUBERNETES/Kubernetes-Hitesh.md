***Kubernetes, also known as k8s is an open-source system for automating deployment, scaling, and management of containerized applications.***

# 1. What is Kubernetes?

Kubernetes is a **container orchestration platform**.

Docker lets you do:

```text
Build image
    ↓
Run container
```

For example:

```bash
docker run my-api
```

But imagine you have:

```text
10 backend containers
5 frontend containers
3 database containers
2 worker containers
```

Now you need to handle:

- Which machine should run each container?
- What happens if a container crashes?
- How do containers find each other?
- How do you expose the application to the internet?
- How do you store passwords?
- How do you scale from 3 containers to 10?
- How do you update the application without downtime?
- What happens if an entire machine dies?

Doing all of this manually becomes difficult.

That's where Kubernetes comes in.


# 2. Docker vs Kubernetes

Think of Docker as the **container engine** and Kubernetes as the **manager**.

## Docker

Docker is mainly concerned with:

```text
Image → Container
```

Example:

```bash
docker build -t my-api .
docker run my-api
```

Docker answers:

> "How do I run this container?"

---

## Kubernetes

Kubernetes answers:

> "How should my application run and stay running?"

For example:

```text
Run 3 copies of my API.

If one crashes:
    start another one.

If traffic increases:
    run more copies.

Make the API accessible to other applications.

Expose the API to the internet.

Give the containers these environment variables.

Update the application gradually.
```

---



```text
                    Kubernetes
                        │
       ┌────────────────┼────────────────┐
       ↓                ↓                ↓
    Deployments       Services         Ingress
       ↓                ↓                ↓
      Pods           Networking      HTTP Routing
       ↓
   Containers
```

---

# 3. The Kubernetes Mental Model

A Kubernetes cluster can be thought of like this:

```text
Kubernetes Cluster
│
├── Node
│   ├── Pod
│   │   └── Container
│   │
│   └── Pod
│       └── Container
│
├── Node
│   ├── Pod
│   │   └── Container
│   │
│   └── Pod
│       └── Container
│
└── Node
    └── Pod
        └── Container
```

---



### Kubectl :-
- It is command line interface to interact with kubernetes
### minikube
- it is a way to interact learn and study kubernetes
- it is very resource consuming 
- to learn in a single container is minikube

![[Pasted image 20260830014354.png]]

`We only ineract with control plane,The control place interacts with worker nodes`

# 5. Node

A **Node** is a machine that runs your application workloads.

For example:

```text
Cluster
│
├── Node 1
│   ├── frontend Pod
│   └── backend Pod
│
├── Node 2
│   ├── backend Pod
│   └── worker Pod
│
└── Node 3
    └── backend Pod
```

A node is basically:

> A machine where Kubernetes can run Pods.

If you're using a local Kubernetes installation such as Minikube, your entire cluster might run inside one machine.

In a cloud cluster, you may have many machines.

---

# 6. Pod

## The Most Important Kubernetes Concept

A **Pod** is the smallest deployable unit in Kubernetes.

This is where beginners often get confused.

You might think:

```text
Kubernetes → Container
```

But actually:

```text
Kubernetes → Pod → Container
```

A Pod usually contains **one container**.

Example:

```text
Pod
└── Container
    └── my-api
```

But a Pod can contain multiple containers:

```text
Pod
├── Container A
└── Container B
```

These containers share:

- Network
    
- IP address
    
- Some storage
    

---

# 7. Why does Kubernetes use Pods?

Why not simply run containers directly?

Because Kubernetes wants to manage an **application unit**, not just an individual container.

A Pod represents:

> "These container(s) belong together and should run together."

For most applications:

```text
1 Pod = 1 Container
```

For example:

```text
backend-pod
└── backend-container

frontend-pod
└── frontend-container
```

Don't overthink multi-container Pods initially.

As a beginner, use:

```text
1 Pod → 1 main container
```

unless you have a specific reason to use multiple containers.

---

# 8. Pod IP

Every Pod gets an IP address.

Example:

```text
Pod A
IP: 10.0.0.5

Pod B
IP: 10.0.0.8
```

But there is a problem.

Pods are **temporary**.

If:

```text
Pod A
IP = 10.0.0.5
```

dies and Kubernetes creates a replacement:

```text
New Pod
IP = 10.0.0.15
```

Now anything using `10.0.0.5` breaks.

So we need something more stable.

That thing is a:



![[Pasted image 20260830014754.png]]

# 9. Service

A **Service** provides a stable network endpoint for Pods.

Think of it as:

> A permanent address in front of constantly changing Pods.

Without Service:

```text
Client
   ↓
Pod
```

Problem:

```text
Pod dies
 ↓
New Pod
 ↓
New IP
```

With Service:

```text
Client
   ↓
Service
   ↓
Pod
```

If the Pod dies:

```text
Client
   ↓
Service
   ↓
New Pod
```

The client doesn't care.

---

# 10. Service + Pods

Suppose you have:

```text
Pod 1
Pod 2
Pod 3
```

All running your backend.

The Service sits in front:

```text
                 Backend Service
                       │
            ┌──────────┼──────────┐
            ↓          ↓          ↓
          Pod 1      Pod 2      Pod 3
```

The Service can distribute traffic between them.

This is one reason Kubernetes can easily run multiple copies of your application.

---

# 11. How does a Service know which Pods belong to it?

Using **labels**.

Pod:

```yaml
metadata:
  labels:
    app: backend
```

Service:

```yaml
selector:
  app: backend
```

The Service essentially says:

> "Send traffic to Pods with `app=backend`."

So:

```text
Service
   │
   │ selector: app=backend
   ↓
┌───────────────┐
│ Pod 1         │ app=backend
│ Pod 2         │ app=backend
│ Pod 3         │ app=backend
└───────────────┘
```

---

# 12. Deployment

![[Pasted image 20260902234222.png]]

Now we have Pods.

But who creates and manages them?

Usually:

# Deployment

A Deployment tells Kubernetes:

> "I want this many copies of this application running."

For example:

```yaml
replicas: 3
```

means:

```text
Deployment
    ↓
┌─────────┬─────────┬─────────┐
│ Pod 1   │ Pod 2   │ Pod 3   │
└─────────┴─────────┴─────────┘
```

If Pod 2 crashes:

```text
Deployment notices:

3 Pods required
2 Pods running

        ↓

Create another Pod
```

Result:

```text
Pod 1
Pod 2
Pod 3
```

The exact Pod identities/IPs can change, but Kubernetes maintains the desired number.

---

# 13. Deployment vs Pod

This distinction is extremely important.

### Pod

A Pod is the thing actually running your container.

```text
Pod
└── Container
```

### Deployment

A Deployment manages Pods.

```text
Deployment
    ↓
Pods
    ↓
Containers
```

You normally don't create individual application Pods manually.

Instead:

```text
Deployment
    ↓
creates/manages Pods
```
# 15. ConfigMap

Applications need configuration.

For example:

```text
PORT=5000
NODE_ENV=production
API_URL=https://api.example.com
```

You could put these directly into your application.

But that's bad because configuration should be separate from application code.

Kubernetes provides:

# ConfigMap

A ConfigMap stores **non-sensitive configuration**.

Example:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: backend-config

data:
  NODE_ENV: production
  PORT: "5000"
  API_URL: https://api.example.com
```

Then your Pod can use these values.

Conceptually:

```text
ConfigMap
│
├── NODE_ENV=production
├── PORT=5000
└── API_URL=...
       ↓
     Pod
       ↓
   Container
```

---

# 16. Secret

What about sensitive information?

For example:

```text
DATABASE_PASSWORD
JWT_SECRET
API_KEY
```

You shouldn't put these in a ConfigMap.

Kubernetes provides:

# Secret

A Secret stores sensitive configuration such as:

```text
Passwords
Tokens
API keys
Credentials
```

Example:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: backend-secret

stringData:
  DATABASE_PASSWORD: mypassword
  JWT_SECRET: mysecret
```

Then:

```text
Secret
   ↓
Pod
   ↓
Container
```

---

# 17. ConfigMap vs Secret

Remember:

|ConfigMap|Secret|
|---|---|
|Non-sensitive configuration|Sensitive configuration|
|PORT|Password|
|NODE_ENV|API key|
|API URL|JWT secret|
|Feature flags|Database credentials|

Simple rule:

> **ConfigMap = configuration**
> 
> **Secret = sensitive configuration**

Important:

Kubernetes Secrets are not automatically equivalent to a highly secure external secrets manager. By default, Kubernetes stores Secret data encoded rather than magically making it secret from anyone with appropriate cluster access.

For production, you may eventually encounter tools such as:

- Cloud secret managers
    
- HashiCorp Vault
    
- External Secrets Operator
    

But learn Kubernetes Secrets first.

---

# 18. Environment Variables

Your application can receive ConfigMap and Secret values as environment variables.

For example:

```text
ConfigMap
    ↓
NODE_ENV=production

Secret
    ↓
DATABASE_PASSWORD=123

Pod
    ↓
Container
    ↓
Environment Variables
```

Your Node.js application could then use:

```javascript
process.env.NODE_ENV
process.env.DATABASE_PASSWORD
```

This is very similar to Docker:

```bash
docker run \
  -e NODE_ENV=production \
  -e DATABASE_PASSWORD=123 \
  my-api
```

Kubernetes simply provides a structured way to manage these values.

---

# 19. Ingress

Now imagine your application is running:

```text
Pod
 ↓
Service
```

The Service can expose your application inside the cluster.

But users on the internet need something like:

```text
https://example.com
```

This is where:

# Ingress

comes in.

Ingress handles **HTTP/HTTPS routing into the cluster**.

The typical flow is:

```text
User
 ↓
Internet
 ↓
Ingress
 ↓
Service
 ↓
Pods
 ↓
Container
```

---

# 20. Why do we need Ingress?

Imagine you have:

```text
Frontend
Backend
Admin
```

You could expose each one separately:

```text
frontend.example.com
backend.example.com
admin.example.com
```

But Ingress can route traffic based on host/path.

For example:

```text
example.com/
        ↓
Frontend Service

example.com/api
        ↓
Backend Service

example.com/admin
        ↓
Admin Service
```

So:

```text
                  Ingress
                     │
          ┌──────────┼──────────┐
          ↓          ↓          ↓
      Frontend     Backend     Admin
      Service      Service     Service
          ↓          ↓          ↓
         Pods       Pods       Pods
```

---

# 21. Ingress Resource vs Ingress Controller

This is another important distinction.

There are two things:

## Ingress Resource

The YAML configuration describing your routing rules.

For example:

```yaml
rules:
  - host: example.com
    http:
      paths:
        - path: /api
          backend:
            service:
              name: backend-service
```

It says:

> "When traffic comes to `/api`, send it to backend-service."

---

## Ingress Controller

The actual software that implements those rules.

Examples include:

- NGINX Ingress Controller
    
- Traefik
    
- HAProxy
    
- Cloud-provider ingress/load-balancing controllers
    

So:

```text
Ingress Resource
      ↓
"Here are my routing rules"
      ↓
Ingress Controller
      ↓
Actually handles traffic
```

Creating an Ingress object alone doesn't necessarily make traffic work. You need an appropriate Ingress Controller.

---

# 22. The Complete Web Application

Now let's put everything together.

Suppose you have:

```text
React frontend
Node.js backend
MongoDB
```

A simplified Kubernetes architecture might look like:

```text
                         INTERNET
                            │
                            ↓
                         Ingress
                       /          \
                      /            \
                     ↓              ↓
              Frontend Service   Backend Service
                     │              │
               ┌─────┴─────┐   ┌────┴────┐
               ↓           ↓   ↓         ↓
            Frontend     Frontend      Backend
              Pod          Pod           Pods
                                          │
                                          ↓
                                   Database Service
                                          │
                                          ↓
                                      Database
```

Configuration:

```text
ConfigMap
    ↓
Frontend / Backend Pods

Secret
    ↓
Backend Pod
    ↓
Database credentials
```

---

# 23. YAML

Kubernetes is commonly configured using YAML files.

Example:

```yaml
apiVersion: apps/v1
kind: Deployment

metadata:
  name: backend

spec:
  replicas: 3

  selector:
    matchLabels:
      app: backend

  template:
    metadata:
      labels:
        app: backend

    spec:
      containers:
        - name: backend
          image: my-backend:1.0
          ports:
            - containerPort: 5000
```

This looks complicated initially.

Read it from the outside in:

```text
kind: Deployment
        ↓
I am creating a Deployment

replicas: 3
        ↓
I want 3 Pods

template
        ↓
This is what each Pod should look like

containers
        ↓
Run this container

image
        ↓
Use this Docker image
```

---

# 24. Kubernetes YAML Mental Model

Almost every Kubernetes YAML starts with:

```yaml
apiVersion: ...
kind: ...
metadata:
...
spec:
...
```

Think:

### `apiVersion`

Which Kubernetes API version is being used?

```yaml
apiVersion: apps/v1
```

### `kind`

What Kubernetes object are we creating?

```yaml
kind: Deployment
```

Could be:

```text
Deployment
Service
Pod
ConfigMap
Secret
Ingress
```

### `metadata`

Information about the object.

```yaml
metadata:
  name: backend
```

### `spec`

What should this object do/look like?

```yaml
spec:
  replicas: 3
```

Think:

```text
apiVersion → Which API?
kind       → What object?
metadata   → What is it called?
spec       → What do I want?
```

---

# 25. Labels

Labels are key-value pairs attached to Kubernetes objects.

Example:

```yaml
labels:
  app: backend
  environment: production
```

Think of them as **tags**.

A Pod might have:

```text
app=backend
```

A Service can say:

```text
Find Pods where app=backend
```

This creates the connection:

```text
Service
 selector:
   app=backend
       ↓
Pod
 label:
   app=backend
```

Labels are extremely important in Kubernetes.

---

# 26. Selector

A selector tells Kubernetes:

> "Which objects am I interested in?"

Example:

```yaml
selector:
  app: backend
```

Meaning:

```text
Find objects with:

app=backend
```

For a Service:

```text
Service
  ↓
selector
  ↓
Pods with matching labels
```

For a Deployment:

```text
Deployment
  ↓
selector
  ↓
Pods it manages
```

---

# 27. Service Types

There are several Service types.

The main ones to understand initially are:

```text
ClusterIP
NodePort
LoadBalancer
```

---

## ClusterIP

Default Service type.

```text
Service
   ↓
Pods
```

Accessible inside the cluster.

Good for:

```text
Backend
Database
Internal services
```

Example:

```text
Frontend Pod
    ↓
Backend Service
    ↓
Backend Pods
```

The frontend can communicate with the backend without exposing the backend directly to the internet.

---

## NodePort

Exposes a Service through a port on each node.

Conceptually:

```text
Internet
   ↓
Node IP:30000
   ↓
Service
   ↓
Pods
```

Useful for learning/testing, but you generally don't use NodePort as the main production HTTP entry point when you have a proper ingress/load-balancer setup.

---

## LoadBalancer

Asks the environment/cloud provider for an external load balancer.

Conceptually:

```text
Internet
   ↓
Cloud Load Balancer
   ↓
Service
   ↓
Pods
```

Common in cloud Kubernetes environments.

---

# 28. Service vs Ingress

This is a common interview question.

### Service

Connects traffic to Pods.

```text
Service
   ↓
Pods
```

### Ingress

Routes external HTTP/HTTPS traffic to Services.

```text
Ingress
   ↓
Service
   ↓
Pods
```

Therefore:

```text
Ingress = HTTP routing
Service = stable access to Pods
```

---

# 29. DNS in Kubernetes

Kubernetes provides internal DNS.

Suppose you have:

```text
backend-service
```

Other Pods can usually access it using its Service DNS name.

For example:

```text
http://backend-service:5000
```

Instead of:

```text
http://10.0.0.23:5000
```

Why?

Because Pod IPs can change.

The Service name remains stable.

So:

```text
Pod IP ❌
Service DNS ✅
```

This is an important Kubernetes rule.

---

# 30. Namespace

A Namespace provides a logical separation inside a cluster.

Think:

```text
Cluster
│
├── development
│   ├── backend
│   ├── frontend
│   └── database
│
├── staging
│   ├── backend
│   └── frontend
│
└── production
    ├── backend
    ├── frontend
    └── database
```

Namespaces are useful for:

- Separating environments
    
- Organizing applications
    
- Access control
    
- Resource management
    

You might have:

```text
dev
staging
production
```

---

# 31. Volume

Containers are generally ephemeral.

Suppose your container writes:

```text
/data/file.txt
```

Then the container is deleted.

The data may disappear with it.

Kubernetes provides storage mechanisms through **Volumes** and related storage resources.

Conceptually:

```text
Pod
 ↓
Volume
 ↓
Persistent Storage
```

For databases, you generally need persistent storage.

---

# 32. PersistentVolume

A **PersistentVolume (PV)** represents storage available to Kubernetes.

Think:

```text
Physical / Cloud Storage
          ↓
PersistentVolume
```

A PV provides storage that can outlive an individual Pod.

---

# 33. PersistentVolumeClaim

A **PersistentVolumeClaim (PVC)** is a request for storage.

Think:

```text
Application:

"I need 10 GB of storage."

          ↓

PVC

          ↓

Kubernetes finds/provisions suitable storage

          ↓

PV
```

Simple distinction:

```text
PV  = storage
PVC = request for storage
```

---

# 34. Stateful vs Stateless

This is useful when thinking about Kubernetes architecture.

## Stateless

The application doesn't need local data to survive.

Example:

```text
Node.js API
```

If the Pod disappears:

```text
Old Pod ❌

New Pod
   ↓
Application continues
```

Good candidate for:

```text
Deployment
```

---

## Stateful

The application needs persistent data/state.

Examples:

```text
Database
Redis with persistence
Message queues
```

These often require:

```text
Persistent Storage
```

and sometimes Kubernetes:

```text
StatefulSet
```

You don't need to master StatefulSets immediately.

---

# 35. StatefulSet

A StatefulSet is designed for applications where Pods have persistent identity and/or storage requirements.

Deployment:

```text
backend-abc123
backend-xzy456
```

Pods are interchangeable.

StatefulSet:

```text
database-0
database-1
database-2
```

Pods have stable identities.

Think:

```text
Deployment
→ interchangeable Pods

StatefulSet
→ identifiable/stateful Pods
```

---

# 36. ReplicaSet

A ReplicaSet ensures a specific number of Pod replicas exist.

For example:

```text
replicas: 3
```

means:

```text
ReplicaSet
   ↓
Pod
Pod
Pod
```

Deployments normally manage ReplicaSets for you.

So your typical mental model is:

```text
Deployment
   ↓
ReplicaSet
   ↓
Pods
```

You usually work with Deployments rather than manually managing ReplicaSets.

---

# 37. Rolling Updates

Suppose your current application is:

```text
backend:v1
```

You want:

```text
backend:v2
```

Kubernetes can gradually replace old Pods.

Instead of:

```text
Delete everything
↓
Start everything
```

it can do:

```text
v1   v1   v1

 ↓

v2   v1   v1

 ↓

v2   v2   v1

 ↓

v2   v2   v2
```

This is a **rolling update**.

It helps reduce downtime during deployments.

---

# 38. Rollback

Suppose:

```text
v1 → v2
```

But v2 is broken.

Kubernetes can roll back the Deployment to an earlier revision.

Conceptually:

```text
v1
 ↓
v2 ❌
 ↓
rollback
 ↓
v1 ✅
```

This is another advantage of using Deployments.

---

# 39. Health Checks

Kubernetes can check whether your application is healthy.

There are three important probe concepts:

```text
Liveness Probe
Readiness Probe
Startup Probe
```

---

## Liveness Probe

Asks:

> "Is this container still alive?"

If the application is stuck, Kubernetes can restart it.

```text
Container
    ↓
Liveness check
    ↓
Failed
    ↓
Restart
```

---

## Readiness Probe

Asks:

> "Is this application ready to receive traffic?"

This is different from being alive.

Example:

```text
Container started
       ↓
Application still loading
       ↓
Not Ready
       ↓
Don't send traffic
```

Once ready:

```text
Ready
 ↓
Service sends traffic
```

---

## Startup Probe

Useful for applications that take a long time to start.

It gives the application time to initialize before normal liveness/readiness behavior becomes important.

---

# 40. Requests and Limits

Kubernetes needs to know how much CPU and memory your containers need.

Example:

```yaml
resources:
  requests:
    cpu: "250m"
    memory: "256Mi"

  limits:
    cpu: "500m"
    memory: "512Mi"
```

### Request

Minimum amount Kubernetes uses when deciding where to schedule the Pod.

Think:

> "I need approximately this much."

### Limit

Maximum amount the container is allowed to use for that resource.

Think:

> "Don't let me go beyond this."

---

# 41. Scheduling

Kubernetes decides which Node should run a Pod.

For example:

```text
Node 1
CPU: available

Node 2
CPU: busy

Node 3
CPU: available
```

Kubernetes scheduler chooses a suitable node.

Conceptually:

```text
Deployment
    ↓
Pod needs to run
    ↓
Scheduler
    ↓
Choose Node
    ↓
Pod starts
```

---

# 42. Control Plane

A Kubernetes cluster has a **control plane** that manages the cluster.

Important components include:

```text
Control Plane
│
├── API Server
├── Scheduler
├── Controller Manager
└── etcd
```

You don't need to memorize every internal component initially, but understand the purpose.

---

# 43. API Server

The Kubernetes API Server is the main entry point to Kubernetes.

When you run:

```bash
kubectl apply -f deployment.yaml
```

you're communicating with the Kubernetes API.

Think:

```text
kubectl
   ↓
API Server
   ↓
Kubernetes
```

The API Server is essentially the front door to the Kubernetes control plane.

---

# 44. etcd

`etcd` stores Kubernetes cluster state.

Think:

```text
Kubernetes needs to remember:

Deployments
Pods
Services
Secrets
ConfigMaps
etc.

        ↓

      etcd
```

You usually don't interact with etcd directly.

For now remember:

> **etcd = Kubernetes' backing store for cluster state.**

---

# 45. Scheduler

The Scheduler decides:

> "Which Node should run this Pod?"

Example:

```text
Pod needs:
CPU = 500m
Memory = 512Mi

          ↓

Scheduler

          ↓

Node 2
```

---

# 46. Controller Manager

Controllers continuously compare:

```text
Desired State
      vs
Actual State
```

Example:

```text
Desired:
3 Pods

Actual:
2 Pods
```

Controller:

```text
Create another Pod
```

This is the mechanism behind Kubernetes' reconciliation behavior.

---

# 47. Worker Node Components

A worker Node typically has components responsible for running workloads, including:

```text
kubelet
container runtime
kube-proxy
```

### kubelet

The kubelet runs on each node and makes sure the Pods assigned to that node are running.

Think:

```text
Control Plane
      ↓
"Run this Pod on Node 1"
      ↓
kubelet
      ↓
Container runtime
      ↓
Container
```

### Container Runtime

Actually runs the containers.

Examples include runtimes based on:

```text
containerd
CRI-O
```

You don't need to think of Kubernetes as requiring the Docker CLI itself to run containers.

### kube-proxy

Helps implement Service networking on nodes.

For beginner-level understanding:

> kube-proxy helps Kubernetes Services route network traffic.

---

# 48. kubectl

`kubectl` is the command-line tool used to interact with Kubernetes.

Think:

```text
kubectl
   ↓
Kubernetes API Server
   ↓
Cluster
```

Common commands:

```bash
kubectl get pods
```

List Pods.

```bash
kubectl get deployments
```

List Deployments.

```bash
kubectl get services
```

List Services.

```bash
kubectl get nodes
```

List Nodes.

---

# 49. Applying YAML

Suppose you have:

```text
deployment.yaml
```

You can apply it:

```bash
kubectl apply -f deployment.yaml
```

Kubernetes reads the YAML and changes the cluster toward the desired state.

To inspect:

```bash
kubectl get pods
```

```bash
kubectl get deployments
```

```bash
kubectl get services
```

---

# 50. Debugging Kubernetes

When something doesn't work, don't randomly change YAML.

Use:

```bash
kubectl get pods
```

Then:

```bash
kubectl describe pod <pod-name>
```

Check logs:

```bash
kubectl logs <pod-name>
```

Execute a command inside a container:

```bash
kubectl exec -it <pod-name> -- sh
```

A useful debugging flow:

```text
Something doesn't work
        ↓
kubectl get pods
        ↓
Is Pod Running?
        ↓
No → kubectl describe pod
        ↓
Yes
        ↓
kubectl logs
        ↓
Check application
        ↓
Check Service
        ↓
Check Ingress
```

---

# 51. Complete Kubernetes Architecture

Now combine everything.

```text
                              INTERNET
                                  │
                                  ↓
                               Ingress
                                  │
                    ┌─────────────┴─────────────┐
                    ↓                           ↓
             Frontend Service            Backend Service
                    │                           │
              ┌─────┴─────┐               ┌────┴────┐
              ↓           ↓               ↓         ↓
         Frontend Pod  Frontend Pod   Backend Pod Backend Pod
                                              │
                                              ↓
                                       Database Service
                                              │
                                              ↓
                                        Database Pods


ConfigMap ──────────────→ Pods
Secret ─────────────────→ Pods
```

Behind all of this:

```text
                    Kubernetes Cluster
                           │
            ┌──────────────┼──────────────┐
            ↓              ↓              ↓
          Node           Node           Node
            │              │              │
           Pods           Pods           Pods
```

And controlling the cluster:

```text
                    Control Plane
                         │
        ┌────────────────┼────────────────┐
        ↓                ↓                ↓
   API Server        Scheduler        Controllers
        │
       etcd
```

---

# 52. Docker → Kubernetes Mapping

This is probably the easiest way for you to transition from Docker.

|Docker Concept|Kubernetes Equivalent|
|---|---|
|Docker Image|Container image|
|`docker run`|Pod/container managed by Kubernetes|
|Container|Container inside Pod|
|Multiple containers|Multiple Pods / replicas|
|Docker network|Kubernetes networking|
|`-e ENV=value`|ConfigMap / Secret|
|Port mapping|Service / Ingress|
|Restart container|Kubernetes controllers|
|Multiple replicas|Deployment|
|Docker Compose|Kubernetes manifests / Helm|
|Docker volume|Kubernetes Volume / PVC|
|Docker host|Kubernetes Node|
|Docker Swarm-style orchestration|Kubernetes orchestration|

---

# 53. Docker Compose vs Kubernetes

If you're familiar with Docker Compose, this comparison helps.

Docker Compose:

```yaml
services:
  backend:
    image: my-backend
    ports:
      - "5000:5000"

  frontend:
    image: my-frontend
    ports:
      - "3000:3000"
```

Kubernetes breaks these responsibilities into separate objects.

For example:

```text
backend
   ↓
Deployment
   ↓
Pods

backend networking
   ↓
Service

backend configuration
   ↓
ConfigMap

backend secrets
   ↓
Secret

external HTTP access
   ↓
Ingress
```

Kubernetes is more verbose because it is designed for much larger and more dynamic environments.

---

# 54. A Real Example

Imagine your application is:

```text
Next.js frontend
Express backend
MongoDB
```

You build:

```text
frontend image
backend image
```

Kubernetes might look like:

```text
                    Internet
                       │
                       ↓
                    Ingress
                   /       \
                  /         \
                 ↓           ↓
          Frontend Service   Backend Service
                 │               │
            ┌────┴────┐      ┌───┴────┐
            ↓         ↓      ↓        ↓
        Frontend   Frontend Backend Backend
          Pod        Pod      Pod      Pod
                               │
                               ↓
                         Mongo Service
                               │
                               ↓
                           MongoDB
```

Configuration:

```text
ConfigMap
├── NODE_ENV
├── API_URL
└── PORT

Secret
├── JWT_SECRET
└── MONGO_PASSWORD
```

Storage:

```text
MongoDB
   ↓
PVC
   ↓
Persistent Storage
```

---

# 55. The Most Important Objects

Don't try to memorize Kubernetes all at once.

Start with these:

```text
Pod
Deployment
Service
ConfigMap
Secret
Ingress
```

Then learn:

```text
Namespace
Volume
PVC
StatefulSet
Probes
Resources
```

Then learn the internals:

```text
API Server
Scheduler
Controller Manager
etcd
kubelet
kube-proxy
```

---

# 56. One-Line Definitions

These are worth memorizing.

> **Cluster** — The entire Kubernetes environment.

> **Node** — A machine in the Kubernetes cluster.

> **Pod** — The smallest deployable unit; usually contains one application container.

> **Container** — The actual application process running inside a Pod.

> **Deployment** — Manages replicated, replaceable Pods and handles updates.

> **ReplicaSet** — Ensures the desired number of Pod replicas exist.

> **Service** — Provides a stable network endpoint for Pods.

> **ConfigMap** — Stores non-sensitive application configuration.

> **Secret** — Stores sensitive configuration such as credentials and tokens.

> **Ingress** — Provides HTTP/HTTPS routing from outside the cluster to Services.

> **Ingress Controller** — Software that actually implements Ingress routing.

> **Namespace** — Logical isolation/grouping within a cluster.

> **Volume** — Storage mounted into a Pod.

> **PV** — Persistent storage available to Kubernetes.

> **PVC** — A request for persistent storage.

> **StatefulSet** — Manages stateful workloads with stable identities/storage.

> **kubectl** — CLI used to communicate with Kubernetes.

---

# 57. The Mental Model to Remember

If you forget everything else, remember this:

```text
                     USER
                       │
                       ↓
                   INGRESS
               "Where should HTTP
                   traffic go?"
                       │
                       ↓
                   SERVICE
               "Which Pods should
                  receive it?"
                       │
                       ↓
                 DEPLOYMENT
              "How many Pods should
                    exist?"
                       │
                       ↓
                     POD
                "Run this workload"
                       │
                       ↓
                  CONTAINER
                "Run my image"
```

Configuration:

```text
ConfigMap ──→ non-sensitive config ──→ Pod
Secret ─────→ sensitive config ───────→ Pod
```

Storage:

```text
Pod
 ↓
PVC
 ↓
Persistent Storage
```

Infrastructure:

```text
Cluster
   ↓
Nodes
   ↓
Pods
   ↓
Containers
```

Control:

```text
kubectl
   ↓
API Server
   ↓
Control Plane
   ↓
Cluster
```

---

# 58. The Full Picture in One Diagram

```text
                         ┌───────────────────┐
                         │     INTERNET      │
                         └─────────┬─────────┘
                                   │
                                   ↓
                         ┌───────────────────┐
                         │      INGRESS      │
                         │   HTTP / HTTPS    │
                         └─────────┬─────────┘
                                   │
                    ┌──────────────┴──────────────┐
                    ↓                             ↓
          ┌─────────────────┐           ┌─────────────────┐
          │ FRONTEND SERVICE │           │  BACKEND SERVICE │
          └────────┬────────┘           └────────┬────────┘
                   │                             │
             ┌─────┴─────┐                 ┌─────┴─────┐
             ↓           ↓                 ↓           ↓
          ┌──────┐    ┌──────┐          ┌──────┐    ┌──────┐
          │ Pod  │    │ Pod  │          │ Pod  │    │ Pod  │
          │      │    │      │          │      │    │      │
          │ FE   │    │ FE   │          │ BE   │    │ BE   │
          └──────┘    └──────┘          └──────┘    └──────┘
                                             │
                                             ↓
                                      ┌──────────────┐
                                      │ DB SERVICE   │
                                      └──────┬───────┘
                                             │
                                             ↓
                                      ┌──────────────┐
                                      │  DATABASE    │
                                      │     POD      │
                                      └──────┬───────┘
                                             │
                                             ↓
                                           PVC
                                             │
                                             ↓
                                    Persistent Storage


      ┌──────────────────┐
      │    CONFIGMAP     │
      │ non-secret config│
      └────────┬─────────┘
               │
               ↓
              Pods


      ┌──────────────────┐
      │      SECRET      │
      │ passwords/tokens │
      └────────┬─────────┘
               │
               ↓
              Pods


              KUBERNETES CLUSTER
              ──────────────────

          ┌─────────────────────────────┐
          │        CONTROL PLANE       │
          │                             │
          │ API Server                  │
          │ Scheduler                   │
          │ Controllers                 │
          │ etcd                        │
          └──────────────┬──────────────┘
                         │
             ┌───────────┼───────────┐
             ↓           ↓           ↓
          ┌──────┐    ┌──────┐    ┌──────┐
          │Node 1│    │Node 2│    │Node 3│
          └──────┘    └──────┘    └──────┘
```

---

# 59. What You Should Learn Next

Once these concepts make sense, learn Kubernetes in this order:

```text
1. Docker
   ↓
2. Kubernetes architecture
   ↓
3. Pods
   ↓
4. Deployments
   ↓
5. Services
   ↓
6. ConfigMaps
   ↓
7. Secrets
   ↓
8. Ingress
   ↓
9. Volumes + PVC
   ↓
10. Namespaces
   ↓
11. Probes
   ↓
12. Resource requests/limits
   ↓
13. StatefulSets
   ↓
14. Jobs / CronJobs
   ↓
15. Helm
   ↓
16. Kubernetes networking
   ↓
17. RBAC
   ↓
18. Production Kubernetes
```

---

# 60. Final Cheat Sheet

```text
KUBERNETES
│
├── Cluster
│   └── Collection of Nodes
│
├── Node
│   └── Machine
│       └── Pods
│           └── Containers
│
├── Deployment
│   └── Manages Pods
│
├── ReplicaSet
│   └── Maintains number of Pods
│
├── Service
│   └── Stable networking → Pods
│
├── Ingress
│   └── HTTP/HTTPS → Services
│
├── ConfigMap
│   └── Non-secret configuration
│
├── Secret
│   └── Sensitive configuration
│
├── Volume
│   └── Storage for Pods
│
├── PVC
│   └── Request for persistent storage
│
├── StatefulSet
│   └── Stateful applications
│
└── Namespace
    └── Logical separation
```

## The single most important flow

```text
Internet
   ↓
Ingress
   ↓
Service
   ↓
Deployment
   ↓
Pod
   ↓
Container
   ↓
Docker Image
```

And remember:

```text
ConfigMap → Configuration
Secret    → Sensitive Configuration
Service   → Networking
Ingress   → External HTTP Routing
Deployment → Pod Management
Pod       → Container Wrapper
Node      → Machine
Cluster   → Collection of Machines
PVC       → Persistent Storage Request
```

> [!tip] The Kubernetes mindset  
> Don't think:
> 
> **"Start this container."**
> 
> Think:
> 
> **"This is the state I want my application to be in. Kubernetes, make reality match that state."**
> 
> That shift—from **imperative container commands** to **declarative desired state**—is the key to understanding Kubernetes.

