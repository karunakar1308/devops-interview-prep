# Technical Scenarios & Deep-Dive Questions

> **Prepare for hands-on technical discussions and architecture questions**

---

## Common Technical Scenarios

### Scenario 1: Azure DevOps Agent Pool is Overwhelmed

**Question:** "We're seeing build queue times spiking during peak hours. Our Azure DevOps agent pool can't keep up. How would you troubleshoot and resolve this?"

**Your Answer:**

**Step 1 - Diagnose:**
- Check agent pool metrics: queue depth, wait times, agent utilization
- Identify which types of builds are queued (capability requirements)
- Look for patterns: time of day, specific teams, build types

**Step 2 - Immediate Fix:**
- Temporarily increase agent count manually
- Check if agents are stuck or offline
- Review agent capabilities - ensure builds aren't over-constrained

**Step 3 - Long-term Solution:**
- Implement auto-scaling based on queue depth metrics
- Use PowerShell + Azure automation to provision/deprovision agents
- Set up different pools for different workload types
- Consider using Microsoft-hosted agents for non-sensitive workloads
- Add monitoring and alerts for queue time thresholds

**Real Example:** "At Fiserv, I implemented auto-scaling that reduced wait times by 60% and saved 45% on costs by decommissioning agents during off-peak hours."

---

### Scenario 2: GitOps Sync Failures in ArgoCD

**Question:** "Your ArgoCD application shows 'OutOfSync' and deployments are failing. How do you troubleshoot?"

**Your Answer:**

**Investigation Steps:**
1. Check ArgoCD UI - what specific resources are out of sync?
2. Review Git commits - was there a recent change?
3. Check Kubernetes events: `kubectl describe` the failing resources
4. Look at ArgoCD application logs
5. Verify RBAC permissions for ArgoCD service account

**Common Causes & Fixes:**
- **Helm value errors:** Validate values file, check templating
- **Resource conflicts:** Another tool (kubectl/Terraform) modified resources
- **RBAC issues:** Grant ArgoCD service account proper permissions
- **CRD dependencies:** Ensure CRDs are applied before dependent resources (use sync waves)
- **Image pull errors:** Check image registry access and secrets

**Prevention:**
- Use sync waves for dependency ordering
- Enable health checks and sync policies appropriately
- Implement pre-sync hooks for validations
- Set up alerts for sync failures

---

### Scenario 3: SonarQube Quality Gate Blocking Production Deploy

**Question:** "A critical hotfix needs to go to production, but SonarQube quality gate is failing. What do you do?"

**Your Answer:**

**Immediate Assessment:**
- Review what's failing: coverage%, bugs, vulnerabilities, code smells?
- Determine severity: Is it a critical security vulnerability or minor code smell?
- Check if it's new code or existing debt

**Decision Framework:**

**If Critical Security Issue:**
- DO NOT bypass - fix the vulnerability first
- Security trumps speed
- Work with team to quickly remediate

**If Minor Quality Issue (code smell, minor coverage drop):**
- Document the bypass reason
- Get approval from tech lead/manager
- Create immediate follow-up ticket
- Temporarily adjust quality gate for this build only (not globally)
- Ensure fix is included in next deploy

**Long-term Fix:**
- Use "new code" strategy so hotfixes don't accumulate debt
- Have separate quality profiles for hotfix vs. regular builds
- Improve pre-production testing to catch issues earlier

**Real Example:** "At J.P. Morgan, we had separate quality gates: strict for features, flexible for emergency hotfixes with mandatory follow-up reviews."

---

### Scenario 4: JFrog Artifactory Storage Crisis

**Question:** "Artifactory is running out of storage space. How do you handle this?"

**Your Answer:**

**Immediate Triage:**
- Check which repositories are consuming most space
- Identify old artifacts and snapshots
- Look for duplicate or unnecessary artifacts

**Cleanup Strategy:**
1. **Aggressive:** Delete old SNAPSHOT builds (keep only X days/weeks)
2. **Medium:** Remove old feature branch artifacts
3. **Conservative:** Archive old release versions to cheaper storage

**Automated Solution:**
```yaml
Retention Policy:
- Snapshots: 30 days
- Feature branches: 60 days
- Release artifacts: Keep all (or 2 years)
- Failed builds: 7 days
```

**Implementation:**
- Use Artifactory cleanup policies
- Create scheduled jobs for automated cleanup
- Set up storage monitoring and alerts (80% threshold)
- Consider tiered storage (hot/warm/cold)

**Prevention:**
- Educate teams on artifact lifecycle
- Remove unused repositories
- Implement naming conventions to auto-identify temp artifacts

---

### Scenario 5: Kubernetes Pod Constantly Restarting

**Question:** "A production pod is in CrashLoopBackOff. Walk me through your debugging process."

**Your Answer:**

**Step-by-Step Debug:**

```bash
# 1. Check pod status and events
kubectl describe pod <pod-name> -n <namespace>

# 2. Check logs
kubectl logs <pod-name> -n <namespace>
kubectl logs <pod-name> -n <namespace> --previous  # Last crash

# 3. Check resource constraints
kubectl top pod <pod-name> -n <namespace>

# 4. Check liveness/readiness probes
# (from describe output)

# 5. Exec into pod (if stays up long enough)
kubectl exec -it <pod-name> -n <namespace> -- /bin/sh
```

**Common Causes:**
1. **Application crashes:** Check logs for errors
2. **OOMKilled:** Increase memory limits
3. **Failed probes:** Adjust probe timeouts or fix healthcheck endpoint
4. **Missing dependencies:** Check configmaps, secrets, PVCs
5. **Image pull errors:** Verify image exists and pull secrets

**Fix Examples:**
- **If OOMKilled:** Increase memory request/limit
- **If probe failing:** Increase `initialDelaySeconds` or `timeoutSeconds`
- **If config missing:** Verify ConfigMap/Secret exists and is mounted correctly

**Prevention:**
- Set appropriate resource requests/limits
- Implement proper health checks
- Test pod startup in staging first
- Use HPA for traffic spikes

---

## Architecture Discussion Questions

### How Would You Design...

#### 1. Multi-Environment CI/CD Pipeline?

**Key Components:**
- **Source:** Git (GitHub/Azure DevOps)
- **Build:** Compile, test, security scan, build Docker image
- **Artifact Storage:** Push to JFrog Artifactory with immutable tags
- **Deployment:** 
  - Dev: Auto-deploy on merge to develop
  - Test: Auto-deploy with smoke tests
  - Staging: Manual approval required
  - Prod: Manual approval + change ticket

**Security Gates:**
- SonarQube quality gate
- Vulnerability scanning (Trivy/Xray)
- Secret scanning
- Dependency check

**GitOps Integration:**
- ArgoCD watches environment-specific Git repos
- Promotion = update image tag in Git
- Rollback = revert Git commit

---

#### 2. Self-Hosted Runner/Agent Architecture?

**Design Considerations:**

**Infrastructure:**
- VM or container-based runners
- Auto-scaling group (scale 2-50)
- Separate pools/groups per environment
- Ephemeral runners (spin up/down per job)

**Security:**
- Runners in private subnet
- Limited outbound internet access
- No inbound access
- Short-lived credentials (OIDC/federated identity)
- Separate runners for prod vs. non-prod

**Monitoring:**
- Queue depth metrics
- Job duration tracking
- Success/failure rates
- Cost per build

**Scaling Logic:**
```
If queue_depth > 10: scale_up()
If avg_queue_time > 5_min: scale_up()
If idle_agents > 5 for 15_min: scale_down()
```

---

#### 3. Artifact Promotion Strategy?

**Promotion Flow:**
```
Build → Dev Repo → Test Repo → Staging Repo → Prod Repo
       (auto)    (auto)       (approval)     (approval+ticket)
```

**Key Principles:**
- **Immutability:** Same artifact (never rebuild)
- **Traceability:** Tag with Git SHA, build ID, timestamp
- **Scanning:** Vulnerability scan at each promotion
- **Approvals:** Automated for dev/test, manual for staging/prod

**Implementation:**
- JFrog virtual repositories for each environment
- Promotion API triggered by CD pipeline
- Metadata updated at each stage
- Failed scans block promotion

---

## "What If" Questions

### Q: What if your CI/CD pipeline is slow?

**Diagnosis:**
- Profile each stage duration
- Identify bottlenecks (usually: tests, security scans, docker builds)

**Optimizations:**
1. **Parallelize:** Run tests/scans concurrently
2. **Cache:** Docker layer caching, dependency caching
3. **Selective:** Only run affected tests
4. **Hardware:** Faster agents for critical pipelines
5. **Incremental:** Build only what changed

### Q: What if ArgoCD is too slow syncing?

**Options:**
- Increase sync frequency (default: 3 min)
- Use webhooks for instant sync on Git push
- Optimize Helm chart complexity
- Use ArgoCD ApplicationSets for large multi-environment setups

### Q: How do you handle secrets in GitOps?

**Options:**
1. **Sealed Secrets:** Encrypt in Git, ArgoCD decrypts in cluster
2. **External Secrets Operator:** Reference Azure Key Vault/HashiCorp Vault
3. **SOPS:** Encrypt values in Git
4. **Kustomize + Secret Generator:** Generate from external source

**Never:** Store plaintext secrets in Git!

---

## Rapid-Fire Technical Questions

**Q: What's the difference between agent pool and agent queue in Azure DevOps?**
A: Agent pool is the collection of agents. Agent queue connects a project to a pool. Multiple projects can use the same pool via different queues.

**Q: How do you secure GitHub self-hosted runners?**
A: Isolated subnet, ephemeral (rebuild after each job), scoped permissions per runner group, no inbound access, short-lived tokens, separate for public vs private repos.

**Q: What's the difference between Helm values and secrets?**
A: Values are configuration, stored in Git. Secrets are sensitive data, should never be in Git - use external secret managers or sealed secrets.

**Q: When would you use sync waves in ArgoCD?**
A: When resources have dependencies - e.g., CRDs before CRs, namespace before resources in it, config before app deployment.

**Q: How do you handle database migrations in GitOps?**
A: Use pre-sync hooks for migrations, ensure idempotency, test rollback scenarios, consider separate migration jobs vs inline with app deployment.

**Q: What's the difference between readiness and liveness probes?**
A: Readiness = is pod ready to serve traffic? (removes from service if fails). Liveness = is pod alive? (restarts pod if fails). Startup probe = initial health check with longer timeout.

---

## Practice Whiteboarding

Be ready to draw/explain:

1. **CI/CD Pipeline Flow** (Git commit → Production)
2. **ArgoCD GitOps Architecture** (Git → ArgoCD → Kubernetes)
3. **Agent Pool Auto-Scaling** (Queue metrics → Scale decision → VM provisioning)
4. **Artifact Promotion Flow** (Build → Dev → Test → Prod repos)
5. **Kubernetes Pod Lifecycle** (Pending → Running → Failed/Succeeded)

---

**Remember:** It's OK to say "I would need to research the specific tool syntax, but the approach would be [...]" - they're testing your problem-solving process, not memorization!
