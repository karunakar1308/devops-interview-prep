# Kubernetes (AKS & GKE) – STAR Stories

## Story – AKS as an enterprise platform

**Situation**  
Teams were deploying containerized applications inconsistently to an AKS cluster, with ad-hoc YAML and fragile manual steps.

**Task**  
Standardize AKS deployments and improve reliability, security, and developer experience.

**Action**

- Designed AKS with separate **system and user node pools**, Azure CNI, and managed identities for secure access to Azure resources.
- Standardized **Helm charts** for common patterns (web APIs, workers, cron jobs) and integrated them with Azure DevOps and ArgoCD GitOps workflows.
- Implemented **network policies, PodSecurity standards**, secrets in Azure Key Vault, and wired cluster metrics/logs into Azure Monitor.
- Documented onboarding steps so new services could be onboarded with minimal hand-holding.

**Result**  
Increased deployment frequency while reducing production incidents. Teams adopted the standard Helm/GitOps model as the default way to ship services.

---

## Story – GKE multi-environment rollout

**Situation**  
Microservices needed consistent deployments across dev, stage, and prod environments on GKE, but each environment behaved differently due to drift and manual changes.

**Task**  
Create a predictable rollout process on GKE with safe progressive delivery and easy rollbacks.

**Action**

- Provisioned GKE clusters with **separate node pools** and **workload identity**, standardizing cluster configuration via Terraform.
- Built pipelines (Cloud Build/Azure DevOps) that deployed to GKE using versioned manifests and Helm charts.
- Implemented **Horizontal Pod Autoscaler, PodDisruptionBudgets, and health probes** to keep services reliable during rollouts.
- Adopted blue-green/canary strategies for high-risk services and documented clear rollback procedures.

**Result**  
Achieved consistent behavior across environments, reduced rollback time dramatically, and gave stakeholders confidence in each release.
