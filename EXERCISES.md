# CKAD Exercises

This file is the exercise roadmap for CKAD preparation.

Use it as a checklist. Each exercise should eventually have its own detailed steps, manifests, verification commands, and cleanup commands.

## Suggested Environment

- Local Kubernetes cluster: `kind`, `minikube`, Docker Desktop Kubernetes, or any lab cluster.
- CLI tools: `kubectl`.
- Default namespace for practice: create a separate namespace for each topic or use `ckad`.

```bash
kubectl create namespace ckad
kubectl config set-context --current --namespace=ckad
```

## Exercise Layout

Recommended structure for each exercise:

```markdown
## Exercise: Short Name

### Goal

What this exercise teaches.

### Task

What to create, update, debug, or delete.

### Verify

Commands to prove the task works.

### Cleanup

Commands to remove created resources.
```

## 1. Kubectl Basics

- [ ] Exercise 1.1: Check cluster access
  - Goal: Confirm `kubectl` can communicate with the cluster.
  - Practice: `kubectl cluster-info`, `kubectl get nodes`, `kubectl get namespaces`.
  - Verify: nodes and namespaces are listed.

- [ ] Exercise 1.2: Create and switch namespaces
  - Goal: Practice namespace isolation.
  - Practice: create `ckad`, switch current context namespace, list resources in that namespace.
  - Verify: `kubectl config view --minify` shows the expected namespace.

- [ ] Exercise 1.3: Use output formats
  - Goal: Extract useful information from resources.
  - Practice: `-o wide`, `-o yaml`, `-o json`, `jsonpath`.
  - Verify: print only selected fields from a Pod or Deployment.

- [ ] Exercise 1.4: Explain Kubernetes resources
  - Goal: Learn how to inspect valid fields during the exam.
  - Practice: `kubectl explain pod.spec.containers`, `kubectl explain deployment.spec`.
  - Verify: find the field path for container image, ports, env vars, and probes.

## 2. Pods

- [ ] Exercise 2.1: Create a Pod imperatively
  - Goal: Create a simple Pod quickly.
  - Practice: `kubectl run nginx --image=nginx`.
  - Verify: `kubectl get pods` and `kubectl describe pod nginx`.

- [ ] Exercise 2.2: Generate a Pod manifest
  - Goal: Create YAML without applying it immediately.
  - Practice: `kubectl run nginx --image=nginx --dry-run=client -o yaml`.
  - Verify: save the manifest and apply it.

- [ ] Exercise 2.3: Run a command inside a Pod
  - Goal: Use `kubectl exec`.
  - Practice: run shell commands inside a container.
  - Verify: confirm files, processes, or environment variables inside the Pod.

- [ ] Exercise 2.4: View Pod logs
  - Goal: Read application logs.
  - Practice: `kubectl logs`, `kubectl logs -f`.
  - Verify: logs are visible for the target container.

- [ ] Exercise 2.5: Multi-container Pod
  - Goal: Understand containers sharing one Pod network namespace.
  - Practice: create a Pod with two containers.
  - Verify: both containers are running in the same Pod.

## 3. Labels, Selectors, and Annotations

- [ ] Exercise 3.1: Add labels to Pods
  - Goal: Organize resources with labels.
  - Practice: create Pods with `app`, `tier`, and `env` labels.
  - Verify: filter with `kubectl get pods -l app=<name>`.

- [ ] Exercise 3.2: Update labels
  - Goal: Modify labels on existing resources.
  - Practice: `kubectl label pod <name> key=value --overwrite`.
  - Verify: selectors return the expected resources.

- [ ] Exercise 3.3: Use annotations
  - Goal: Store non-identifying metadata.
  - Practice: add annotations to a Pod or Deployment.
  - Verify: inspect annotations in YAML output.

## 4. Deployments and ReplicaSets

- [ ] Exercise 4.1: Create a Deployment
  - Goal: Run a stateless application.
  - Practice: create an `nginx` Deployment with 3 replicas.
  - Verify: 3 Pods are running.

- [ ] Exercise 4.2: Scale a Deployment
  - Goal: Change application capacity.
  - Practice: scale from 3 replicas to 5, then down to 2.
  - Verify: Pod count matches the replica count.

- [ ] Exercise 4.3: Update an image
  - Goal: Perform a rolling update.
  - Practice: update the container image version.
  - Verify: `kubectl rollout status deployment/<name>`.

- [ ] Exercise 4.4: Roll back a Deployment
  - Goal: Recover from a bad rollout.
  - Practice: update to a wrong image, inspect failure, roll back.
  - Verify: Pods return to a healthy image.

- [ ] Exercise 4.5: Inspect ReplicaSets
  - Goal: Understand how Deployments manage ReplicaSets.
  - Practice: list ReplicaSets before and after a rollout.
  - Verify: old and new ReplicaSets are visible.

## 5. Services and Networking

- [ ] Exercise 5.1: Expose a Deployment with ClusterIP
  - Goal: Create stable internal access to Pods.
  - Practice: create a Service for a Deployment.
  - Verify: Service endpoints point to the correct Pods.

- [ ] Exercise 5.2: Test Service access from another Pod
  - Goal: Validate in-cluster networking.
  - Practice: run a temporary client Pod and curl the Service.
  - Verify: request reaches the application.

- [ ] Exercise 5.3: Use NodePort
  - Goal: Expose an application through node ports.
  - Practice: create a `NodePort` Service.
  - Verify: inspect assigned node port.

- [ ] Exercise 5.4: Understand DNS names
  - Goal: Use Kubernetes DNS.
  - Practice: access a Service by short name and full DNS name.
  - Verify: both names resolve from inside the namespace.

## 6. ConfigMaps and Secrets

- [ ] Exercise 6.1: Create a ConfigMap from literals
  - Goal: Store non-sensitive configuration.
  - Practice: create a ConfigMap with app settings.
  - Verify: inspect the ConfigMap.

- [ ] Exercise 6.2: Use ConfigMap as environment variables
  - Goal: Inject config into a container.
  - Practice: reference ConfigMap keys as env vars.
  - Verify: `kubectl exec` shows the environment variables.

- [ ] Exercise 6.3: Mount ConfigMap as files
  - Goal: Provide config files to a container.
  - Practice: mount ConfigMap data into a volume.
  - Verify: files exist inside the container.

- [ ] Exercise 6.4: Create and use a Secret
  - Goal: Store sensitive values.
  - Practice: create a Secret and inject it as env vars.
  - Verify: the application receives the secret value.

- [ ] Exercise 6.5: Mount Secret as files
  - Goal: Use Secret data as mounted files.
  - Practice: mount a Secret into a Pod.
  - Verify: secret files exist with expected contents.

## 7. Volumes and Storage

- [ ] Exercise 7.1: Use `emptyDir`
  - Goal: Share temporary storage between containers in a Pod.
  - Practice: write from one container and read from another.
  - Verify: both containers can access the shared file.

- [ ] Exercise 7.2: Use a PersistentVolumeClaim
  - Goal: Request persistent storage.
  - Practice: create a PVC and mount it into a Pod.
  - Verify: Pod can write to the mounted path.

- [ ] Exercise 7.3: Understand container restarts and data
  - Goal: Compare container filesystem and volume persistence.
  - Practice: write data inside and outside a mounted volume, then restart the Pod.
  - Verify: volume data remains where expected.

## 8. Probes and Application Health

- [ ] Exercise 8.1: Add a readiness probe
  - Goal: Control when a Pod receives traffic.
  - Practice: configure HTTP or command-based readiness probe.
  - Verify: Service only sends traffic to ready Pods.

- [ ] Exercise 8.2: Add a liveness probe
  - Goal: Restart unhealthy containers automatically.
  - Practice: configure a liveness probe that can fail.
  - Verify: restart count increases after failure.

- [ ] Exercise 8.3: Add a startup probe
  - Goal: Protect slow-starting applications.
  - Practice: configure startup and liveness probes together.
  - Verify: app is not killed before startup completes.

## 9. Resource Requests and Limits

- [ ] Exercise 9.1: Set CPU and memory requests
  - Goal: Define minimum resources for scheduling.
  - Practice: add `resources.requests`.
  - Verify: inspect the Pod resource settings.

- [ ] Exercise 9.2: Set CPU and memory limits
  - Goal: Restrict maximum resource usage.
  - Practice: add `resources.limits`.
  - Verify: inspect the Pod resource settings.

- [ ] Exercise 9.3: Troubleshoot unschedulable Pods
  - Goal: Understand scheduling failures.
  - Practice: request more resources than available.
  - Verify: inspect Pending status and events.

## 10. Jobs and CronJobs

- [ ] Exercise 10.1: Create a Job
  - Goal: Run a one-time task.
  - Practice: create a Job that prints a message and exits.
  - Verify: Job completes successfully and logs are available.

- [ ] Exercise 10.2: Configure Job retries
  - Goal: Control retry behavior.
  - Practice: set `backoffLimit`.
  - Verify: failed Pods stop retrying after the limit.

- [ ] Exercise 10.3: Create a CronJob
  - Goal: Run a task on a schedule.
  - Practice: create a CronJob that runs every minute.
  - Verify: Jobs are created by the CronJob.

- [ ] Exercise 10.4: Suspend and resume a CronJob
  - Goal: Control scheduled execution.
  - Practice: set `suspend: true`, then resume.
  - Verify: no new Jobs are created while suspended.

## 11. Security Contexts and Service Accounts

- [ ] Exercise 11.1: Run a container as non-root
  - Goal: Apply basic container security.
  - Practice: configure `runAsNonRoot` and `runAsUser`.
  - Verify: process runs with the expected UID.

- [ ] Exercise 11.2: Set read-only root filesystem
  - Goal: Restrict container writes.
  - Practice: configure `readOnlyRootFilesystem`.
  - Verify: writing to the root filesystem fails.

- [ ] Exercise 11.3: Create and use a ServiceAccount
  - Goal: Assign a ServiceAccount to a Pod.
  - Practice: create a ServiceAccount and reference it in Pod spec.
  - Verify: Pod uses the expected ServiceAccount.

## 12. Ingress

- [ ] Exercise 12.1: Create an Ingress rule
  - Goal: Route HTTP traffic to a Service.
  - Practice: create an Ingress for a sample app.
  - Verify: request reaches the app through the Ingress path or host.

- [ ] Exercise 12.2: Route by path
  - Goal: Send different paths to different Services.
  - Practice: configure `/app1` and `/app2`.
  - Verify: each path reaches the correct backend.

- [ ] Exercise 12.3: Troubleshoot Ingress
  - Goal: Debug common routing issues.
  - Practice: inspect Ingress, Service, endpoints, and Pods.
  - Verify: traffic works after fixing labels, ports, or paths.

## 13. Observability and Troubleshooting

- [ ] Exercise 13.1: Describe broken Pods
  - Goal: Use events to find failures.
  - Practice: troubleshoot image pull errors and crash loops.
  - Verify: identify the root cause from `kubectl describe`.

- [ ] Exercise 13.2: Debug logs
  - Goal: Use logs for application troubleshooting.
  - Practice: inspect current and previous container logs.
  - Verify: find the error message causing failure.

- [ ] Exercise 13.3: Debug Service connectivity
  - Goal: Diagnose why a Service does not reach Pods.
  - Practice: check selectors, endpoints, target ports, and Pod labels.
  - Verify: Service endpoints are populated and curl succeeds.

- [ ] Exercise 13.4: Use temporary debug Pods
  - Goal: Test networking and DNS from inside the cluster.
  - Practice: run a temporary BusyBox or curl Pod.
  - Verify: DNS and HTTP requests work.

## 14. Manifest Editing Speed

- [ ] Exercise 14.1: Generate YAML from imperative commands
  - Goal: Create manifests quickly during the exam.
  - Practice: use `--dry-run=client -o yaml`.
  - Verify: generated YAML applies successfully.

- [ ] Exercise 14.2: Patch resources
  - Goal: Make quick changes without full file edits.
  - Practice: use `kubectl patch`.
  - Verify: resource field changed correctly.

- [ ] Exercise 14.3: Edit live resources
  - Goal: Practice `kubectl edit`.
  - Practice: edit replicas, image, labels, or env vars.
  - Verify: changes are applied and workload is healthy.

## 15. CKAD Mixed Scenarios

- [ ] Exercise 15.1: Deploy and expose an app
  - Goal: Combine Deployment, labels, and Service.
  - Practice: create a Deployment, expose it, and test access.
  - Verify: client Pod can reach the Service.

- [ ] Exercise 15.2: Configure app settings
  - Goal: Combine ConfigMap, Secret, and environment variables.
  - Practice: inject both non-sensitive and sensitive values.
  - Verify: app can read the expected values.

- [ ] Exercise 15.3: Add health checks and resources
  - Goal: Make an app production-like.
  - Practice: configure probes, requests, and limits.
  - Verify: rollout succeeds and Pods are ready.

- [ ] Exercise 15.4: Fix a broken application
  - Goal: Practice exam-style troubleshooting.
  - Practice: repair bad image, wrong port, wrong selector, or missing config.
  - Verify: Deployment is available and Service traffic works.

- [ ] Exercise 15.5: Timed practice set
  - Goal: Simulate exam pressure.
  - Practice: complete 5 to 8 mixed tasks in 30 minutes.
  - Verify: all resources match the requested state.

## Progress Tracker

| Area | Done | Notes |
| --- | --- | --- |
| Kubectl Basics | 0/4 | |
| Pods | 0/5 | |
| Labels, Selectors, and Annotations | 0/3 | |
| Deployments and ReplicaSets | 0/5 | |
| Services and Networking | 0/4 | |
| ConfigMaps and Secrets | 0/5 | |
| Volumes and Storage | 0/3 | |
| Probes and Application Health | 0/3 | |
| Resource Requests and Limits | 0/3 | |
| Jobs and CronJobs | 0/4 | |
| Security Contexts and Service Accounts | 0/3 | |
| Ingress | 0/3 | |
| Observability and Troubleshooting | 0/4 | |
| Manifest Editing Speed | 0/3 | |
| CKAD Mixed Scenarios | 0/5 | |
