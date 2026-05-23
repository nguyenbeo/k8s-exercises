# k8s-exercises

Kubernetes basic concepts and hands-on exercises for CKAD preparation.

## What is K8s?

K8s is short for Kubernetes. Kubernetes is an open-source container orchestration platform.

It helps you run containerized applications across a group of machines called a cluster. Instead of manually starting containers on individual servers, you describe the desired state of your application, and Kubernetes works to keep the real system matching that state.

For example, you can tell Kubernetes:

- Run 3 copies of this application.
- Restart a container if it crashes.
- Expose the application inside or outside the cluster.
- Mount configuration, secrets, or storage into the application.
- Roll out a new version without stopping everything at once.

## Why?

Kubernetes is useful because modern applications often need to be reliable, scalable, portable, and easy to update.

Main reasons to use Kubernetes:

- **Self-healing**: restarts failed containers and replaces unhealthy Pods.
- **Scaling**: increases or decreases the number of running application instances.
- **Service discovery**: lets applications find and communicate with each other.
- **Rolling updates**: updates applications gradually with less downtime.
- **Configuration management**: separates app configuration and secrets from container images.
- **Portability**: runs on local machines, cloud providers, or on-premise infrastructure.
- **Declarative management**: lets you define the desired state in YAML files.

For CKAD, the focus is not on managing the whole cluster. The focus is on designing, deploying, configuring, observing, and troubleshooting applications running on Kubernetes.

## Kubernetes Architecture

```text
                         User / Developer
                               |
                               | kubectl / YAML / API request
                               v
+---------------------------------------------------------------+
|                        Control Plane                          |
|                                                               |
|  +----------------+      +----------------+                   |
|  | kube-apiserver |<---->|      etcd      |                   |
|  +----------------+      +----------------+                   |
|          ^                                                    |
|          |                                                    |
|          v                                                    |
|  +----------------+      +----------------+                   |
|  |   scheduler    |      | controller mgr |                   |
|  +----------------+      +----------------+                   |
|          |                       |                            |
|          | decides where         | watches desired state      |
|          | Pods should run       | and fixes differences      |
+----------|-----------------------|----------------------------+
           |                       |
           v                       v
+---------------------------------------------------------------+
|                         Worker Node 1                         |
|                                                               |
|  +----------------+      +----------------+                   |
|  |    kubelet     |<---->| container      |                   |
|  |                |      | runtime        |                   |
|  +----------------+      +----------------+                   |
|          |                                                    |
|          v                                                    |
|  +---------------------------------------------------------+  |
|  |                         Pods                            |  |
|  |   +------------+    +------------+    +------------+    |  |
|  |   | Container  |    | Container  |    | Container  |    |  |
|  |   +------------+    +------------+    +------------+    |  |
|  +---------------------------------------------------------+  |
|                                                               |
|  +----------------+                                           |
|  |  kube-proxy    |  handles Service networking               |
|  +----------------+                                           |
+---------------------------------------------------------------+
           |
           | more worker nodes can run more Pods
           v
+---------------------------------------------------------------+
|                         Worker Node 2                         |
|                                                               |
|  kubelet + container runtime + kube-proxy + Pods              |
+---------------------------------------------------------------+

External traffic
      |
      v
+-------------+        +-------------+        +-------------+
|   Ingress   | -----> |   Service   | -----> |    Pods     |
+-------------+        +-------------+        +-------------+
```

Key idea:

- The **control plane** makes decisions and stores cluster state.
- The **worker nodes** run application workloads.
- The **kube-apiserver** is the main entry point for users and cluster components.
- **etcd** stores the cluster state.
- The **scheduler** chooses which node should run a Pod.
- The **controller manager** watches the cluster and tries to keep the desired state true.
- The **kubelet** runs on each node and manages Pods on that node.
- The **container runtime** runs containers.
- **kube-proxy** helps Services send traffic to the right Pods.

## Basic Concepts

### Cluster

A Kubernetes cluster is a group of machines used to run applications.

It contains:

- **Control plane**: manages the cluster and makes scheduling decisions.
- **Worker nodes**: run the application workloads.

### Node

A Node is a machine in the cluster. It can be a virtual machine or a physical server.

Worker nodes run Pods.

### Pod

A Pod is the smallest deployable unit in Kubernetes.

A Pod usually runs one container, but it can run multiple tightly related containers that need to share networking and storage.

### Container

A container packages an application with its runtime and dependencies.

Kubernetes does not build the container image itself during normal operation. It pulls an image from a container registry and runs it inside a Pod.

### Deployment

A Deployment manages a set of identical Pods.

It is commonly used for stateless applications. A Deployment helps with:

- Creating Pods.
- Scaling Pods.
- Replacing failed Pods.
- Rolling out new versions.
- Rolling back to older versions.

### ReplicaSet

A ReplicaSet makes sure a specified number of Pod replicas are running.

Deployments usually create and manage ReplicaSets for you, so you normally work with Deployments directly.

### Service

A Service provides a stable network address for reaching Pods.

Pods can be created, deleted, and replaced, so their IP addresses are not stable. A Service gives other applications a consistent way to connect to them.

Common Service types:

- **ClusterIP**: exposes the Service inside the cluster.
- **NodePort**: exposes the Service on a port of each node.
- **LoadBalancer**: exposes the Service using an external load balancer, usually in cloud environments.

### Namespace

A Namespace is a way to divide cluster resources into logical groups.

Namespaces are useful for separating environments, teams, or exercises.

### ConfigMap

A ConfigMap stores non-sensitive configuration data.

Examples:

- Environment names.
- Feature flags.
- Application settings.
- Config files.

### Secret

A Secret stores sensitive configuration data.

Examples:

- Passwords.
- API tokens.
- TLS certificates.

Secrets are encoded by default, but encoding is not the same as encryption. They still need to be handled carefully.

### Volume

A Volume provides storage to a Pod.

Containers are temporary. If a container restarts, data inside the container filesystem can be lost. Volumes allow data to live outside the container lifecycle.

### Job

A Job runs a task until it completes successfully.

Examples:

- Database migration.
- Batch processing.
- One-time script.

### CronJob

A CronJob runs Jobs on a schedule.

Examples:

- Nightly cleanup.
- Scheduled report generation.
- Periodic backups.

### Ingress

Ingress manages external HTTP or HTTPS access to Services inside the cluster.

It is commonly used to route traffic by hostname or path.

## Analogy

Think of Kubernetes like a restaurant kitchen.

- The **cluster** is the whole restaurant kitchen.
- The **control plane** is the head chef or kitchen manager deciding what needs to happen.
- The **worker nodes** are the cooking stations where work actually gets done.
- A **Pod** is one prepared work unit at a station.
- A **container** is the actual cook or tool doing the task inside that work unit.
- A **Deployment** is the recipe instruction that says how many copies of a dish should always be prepared.
- A **ReplicaSet** is the helper that checks whether the correct number of dishes are being prepared.
- A **Service** is the waiter counter: customers do not need to know which exact cook prepared the dish; they just use the stable counter.
- A **ConfigMap** is the public recipe note, like cooking temperature or serving size.
- A **Secret** is private information, like the safe code or supplier password.
- A **Volume** is the pantry or fridge where ingredients can remain even if a cooking station is reset.
- A **Job** is a one-time kitchen task, like preparing a special batch of sauce.
- A **CronJob** is a scheduled task, like cleaning the ovens every night.
- An **Ingress** is the front door and host stand that routes guests to the correct service area.

In short: you describe what should be running, and Kubernetes coordinates the kitchen to keep the desired work happening.
