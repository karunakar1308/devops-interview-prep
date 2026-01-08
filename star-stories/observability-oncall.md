# Observability & On-Call – STAR Stories

## Story – Resolving a Kubernetes platform incident impacting CI/CD

**Situation**  
Multiple deployments to a production Kubernetes cluster started failing, and CI/CD pipelines reported timeouts and image pull errors during rollout.

**Task**  
Lead the investigation, restore successful deployments, and harden the platform to prevent similar incidents.

**Action**

- Correlated Azure DevOps pipeline logs with **Kubernetes events and cluster metrics** to narrow down the issue to node resource pressure and container registry throttling.
- Applied fixes: adjusted **resource requests/limits**, increased node pool capacity, and configured a **local image cache** to reduce registry calls.
- Fixed RBAC for affected service accounts and added **PodDisruptionBudgets** and liveness/readiness probes to make workloads more resilient.
- Created runbooks and added alerts on error patterns (image pull failures, node pressure, deployment timeouts).

**Result**  
Restored successful deployments the same day, improved platform stability, and reduced the frequency of similar incidents in subsequent quarters.
