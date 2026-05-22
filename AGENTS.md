# AGENTS.md

Guidance for future agents working in this repository.

## Project Purpose

This repository is for hands-on Kubernetes exercises while preparing for the CKAD certification.

Keep the content practical, beginner-friendly, and focused on application developers using Kubernetes. The primary audience is someone learning Kubernetes concepts, commands, YAML manifests, and troubleshooting workflows.

## Repository Style

- Use clear, simple English.
- Prefer concise explanations followed by practical examples.
- Keep Markdown readable in a terminal and on GitHub.
- Use ASCII characters unless an existing file clearly uses another style.
- Avoid unnecessary theory when a hands-on explanation is enough.
- When adding commands, prefer commands that work with standard `kubectl`.
- When adding YAML, use valid Kubernetes manifests and include only fields needed for the exercise.

## CKAD Focus

Favor topics that help with CKAD-style application tasks:

- Pods
- Deployments
- ReplicaSets
- Services
- ConfigMaps
- Secrets
- Volumes
- Jobs
- CronJobs
- Ingress
- Namespaces
- Labels and selectors
- Resource requests and limits
- Probes
- Security contexts
- Service accounts
- Troubleshooting with `kubectl`

Avoid going too deep into cluster administration topics unless they directly support application development. Examples of lower-priority topics:

- Installing production clusters
- Managing control plane components
- Deep networking internals
- Cloud-provider-specific cluster operations

## Exercise Guidelines

When adding exercises:

- State the goal clearly.
- Include the expected starting point.
- Include the commands or manifest files needed to complete the task.
- Add a verification step using `kubectl`.
- Add a cleanup step when resources are created.
- Prefer small exercises that teach one or two concepts at a time.

Recommended exercise structure:

```markdown
## Exercise: Short Name

### Goal

What the learner will practice.

### Task

What to create or change.

### Verify

Commands to confirm the result.

### Cleanup

Commands to remove created resources.
```

## Markdown Conventions

- Use `#` for the repository title.
- Use `##` for main sections.
- Use `###` for subsections inside a topic or exercise.
- Wrap commands and Kubernetes object names in backticks.
- Use fenced code blocks with language hints:

```bash
kubectl get pods
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: example
spec:
  containers:
    - name: nginx
      image: nginx:1.25
```

## Validation

When changing manifests or exercises, validate as much as possible locally.

Useful checks:

- Read the edited Markdown for formatting issues.
- Use `kubectl apply --dry-run=client -f <file>` for manifests when a Kubernetes client is available.
- Use `kubectl explain <resource>` to confirm field names when uncertain.

If validation cannot be run because tooling or a cluster is unavailable, mention that clearly in the final response.

## Git Safety

- Do not revert user changes unless explicitly asked.
- Keep edits scoped to the requested topic.
- Do not introduce unrelated restructuring.
- Preserve existing files and naming conventions when adding new material.
