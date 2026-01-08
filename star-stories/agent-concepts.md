# Build Agents – Comprehensive Concepts Guide

A reference guide for understanding Azure DevOps agents, GitHub runners, and CI/CD execution environments.

---

## Core Agent Concepts

### What is a Build Agent?

A **build agent** is a compute resource (physical machine, VM, or container) that runs CI/CD pipeline jobs. The agent executes the tasks defined in your pipeline, such as:

- Compiling code
- Running tests
- Building Docker images
- Deploying applications
- Running scripts and automation

### Agent vs. Agent Pool

- **Agent**: A single compute resource that can execute jobs.
- **Agent Pool**: A logical grouping of agents that share similar capabilities (OS, installed tools, etc.).

**Example**: You might have:
- `Linux-Ubuntu-Pool` with 10 Linux agents
- `Windows-Server-Pool` with 5 Windows agents

---

## Types of Agents

### 1. Microsoft-Hosted Agents (Cloud-Managed)

**What they are**: Agents hosted and managed by Microsoft/GitHub in the cloud.

**Characteristics**:
- Fresh VM created for each job
- No setup or maintenance required
- Pre-installed tools (Java, Python, Node, .NET, etc.)
- Automatically cleaned up after job completion
- Limited to concurrent job limits per account

**Best for**:
- Quick prototyping
- Public projects
- Teams without specific infrastructure needs
- Jobs that don't require persistent state

**Cost Model**:
- Free tier: Limited minutes per month
- Paid: Per additional concurrent job

**Examples**:
- Azure Pipelines: `ubuntu-latest`, `windows-latest`, `macos-latest`
- GitHub Actions: `ubuntu-latest`, `windows-latest`, `macos-latest`

---

### 2. Self-Hosted Agents

**What they are**: Agents running on your own infrastructure (on-premises or private cloud).

**Characteristics**:
- You own and manage the hardware/VM
- Agents are long-lived and reused across jobs
- Can customize tools, OS, and environment
- Persistent state between jobs (unless cleaned)
- Full control over security and access
- Can be scaled on-demand

**Best for**:
- Private/regulated environments
- Jobs requiring specific tools or large datasets
- Persistent data or caching needs
- Long-running workloads
- Proprietary or sensitive code

**Cost Model**:
- Infrastructure costs (VMs, power, maintenance)
- No per-job licensing fees
- Economies of scale with many jobs

---

## Agent Capabilities & Tags

### What are Capabilities?

**Capabilities** describe what an agent can do. They're used to route jobs to appropriate agents.

**Types of Capabilities**:

1. **User-Defined Capabilities**
   - Manually added by administrators
   - Example: `Docker=true`, `Kubernetes=1.24`, `Node=18.0`

2. **System Capabilities** (Auto-discovered)
   - Automatically detected when agent starts
   - Example: `java`, `python`, `npm`, `git`, `docker`

### Example Capability Matching

```
Pipeline Step Requirement:
  - demands:
      - Docker
      - Kubernetes
      - Node >= 16

Agent Pool:
  - Agent 1: Docker=true, Kubernetes=1.24, Node=18 ✅ Matches
  - Agent 2: Docker=false, Python=3.9 ❌ Doesn't match
```

**Why Capabilities Matter**:
- Prevents job failures from missing tools
- Distributes load efficiently
- Ensures right agent is used for right job
- Critical for complex/diverse workloads

---

## Agent Pool Strategies

### 1. Shared Pool (Monolithic)

**Structure**: All agents mixed in one pool

**Pros**:
- Simple setup
- High utilization
- Flexible routing

**Cons**:
- Noisy neighbor problem (one team's heavy job slows others)
- No resource guarantees
- Harder to optimize
- Single point of failure

---

### 2. Dedicated Pools (Segregated)

**Structure**: Separate pools for different teams/workloads

**Pools**:
- `Team-A-Pool`: For Team A's projects
- `Team-B-Pool`: For Team B's projects
- `CI-Pool`: For CI builds
- `Deploy-Pool`: For deployments

**Pros**:
- Resource isolation and guarantees
- Team autonomy
- Easy to scale per team
- No noisy neighbor issues

**Cons**:
- More complex to manage
- Potential underutilization
- More VMs/costs

---

### 3. Hybrid Strategy

**Structure**: Dedicated pools + shared fallback

**Example**:
- `High-Priority-Pool`: Critical workloads
- `Standard-Pool`: General builds (shared, auto-scaling)
- `Specialized-Pool`: GPU, large memory, specific tools

---

## Agent Lifecycle & Management

### Agent States

1. **Healthy/Online**
   - Agent is running and can accept jobs
   - Regularly heartbeats to server

2. **Offline**
   - Agent crashed or lost connection
   - Will not receive new jobs
   - May timeout and be marked as unavailable

3. **Disabled**
   - Administratively disabled
   - Will not receive new jobs
   - Existing jobs may continue or be stopped

4. **Draining** (Advanced)
   - Not accepting new jobs
   - Finishing in-flight jobs
   - Then shutting down cleanly

### Critical Agent Management Operations

**1. Scaling Agents Up**
```
When: Pipeline jobs queuing up, slow throughput
How:  Add more VMs to pool
Timing: Takes 5-10 min per new agent to be ready
```

**2. Scaling Agents Down**
```
When: Underutilization, cost optimization
How:  Drain agents, then terminate VMs
Warning: Don't kill agents mid-job!
```

**3. Rolling Updates/Patching**
```
Steps:
  1. Drain agent from rotation
  2. Wait for in-flight jobs to complete
  3. Apply patches, update tools
  4. Re-enable agent
  5. Repeat for next agent
  
Goal: Zero downtime, all jobs complete
```

**4. Agent Diagnostics**
```
When troubleshooting:
- Check agent logs (usually: _work/_diag/)
- Verify network connectivity to server
- Check disk space (agents can fill up)
- Verify service account permissions
- Test credentials and token expiry
```

---

## Performance & Optimization

### Queue Time vs. Execution Time

**Queue Time**: Time job waits before finding an available agent

```
Formula: Queue Time = (Jobs Waiting) / (Available Agents)

Example:
- 10 jobs queued, 1 agent available → ~10 min queue time
- 10 jobs queued, 5 agents available → ~2 min queue time
```

**Solution**: Autoscaling or increase pool size

### Agent Warming & Caching

**Cold Start Problem**:
- Microsoft-hosted agents: Fresh VM each time (~1-2 min overhead)
- Self-hosted agents: Reused, faster startup

**Caching Strategies**:
- Docker layer caching
- npm/Maven dependency caching
- Artifact caching between jobs
- Pre-warm agents with common tools

**SLA Example**:
```
Target: 95% of jobs start within 2 minutes

If not met:
  ❌ Add agents
  ✅ Optimize job dependencies
  ✅ Implement caching
```

---

## Security & Compliance

### Agent Access Control

**Key Concepts**:

1. **Service Account**
   - Account agent runs as (e.g., `devops-agent` user)
   - Permissions determine what agent can access
   - Should have minimal required permissions

2. **Personal Access Tokens (PAT)**
   - Token agent uses to communicate with Azure DevOps/GitHub
   - Should be scoped to minimal permissions needed
   - Token expiry: 1 year (or custom)
   - Rotate regularly for security

3. **Agent Authentication**
   - Agent proves identity with PAT
   - Server verifies PAT is valid and not expired
   - All communication is over HTTPS/TLS

### Best Practices

```
✅ DO:
- Rotate tokens every 90 days
- Isolate sensitive data on dedicated agents
- Use separate accounts for test vs. prod agents
- Monitor agent access logs
- Remove agents from pools when decommissioning

❌ DON'T:
- Commit PATs to repos
- Use shared/generic service accounts
- Keep agents online after jobs complete
- Grant agents more permissions than needed
```

---

## Common Patterns & Anti-Patterns

### ✅ Good Patterns

1. **Agent Pool per Tier**
   - Separate dev, staging, production agent pools
   - Different security policies per pool

2. **Auto-Scaling Pools**
   - Scale up when queue builds
   - Scale down during off-hours
   - Cost efficient + responsive

3. **Immutable Agent Images**
   - Use Packer/Terraform to build base images
   - Bake tools into image, don't install during jobs
   - Faster job startup, consistent environment

4. **Health Checks & Monitoring**
   - Agents report metrics (utilization, failures)
   - Alerts on high failure rates
   - Automated recovery/replacement

---

### ❌ Anti-Patterns

1. **Shared Monolithic Pool**
   - All projects, all teams, same agents
   - Noisy neighbors, unpredictable performance
   - Hard to scale or troubleshoot

2. **Manual, Pets-Not-Cattle Agents**
   - Manual setup, snowflake configurations
   - Difficult to reproduce or replace
   - Brittle, hard to troubleshoot

3. **No Monitoring or SLAs**
   - Don't know if agents are healthy
   - Queue times creep up gradually
   - Teams blame DevOps when slow

4. **Agents Running Forever**
   - Disk fills up, memory leaks accumulate
   - Zombie processes from failed jobs
   - Configuration drift over time

---

## Interview Key Talking Points

### When Asked: "What are build agents?"

**Answer Structure**:
1. **Definition**: Compute resources that run CI/CD jobs
2. **Types**: Microsoft-hosted vs. self-hosted
3. **Why it matters**: Performance, security, cost
4. **Trade-offs**: Managed vs. control

### When Asked: "How do you scale agent pools?"

**Answer**:
- Monitor queue time and job throughput
- Implement autoscaling (scale sets, Kubernetes)
- Right-size based on demand curves
- Consider cost vs. performance SLAs
- Use dedicated pools for critical workloads

### When Asked: "What's your experience with agent troubleshooting?"

**Answer**:
- Check agent logs and heartbeat status
- Verify network connectivity and credentials
- Monitor disk space and resource utilization
- Implement health checks and recovery
- Automate remediation where possible

---

## Azure DevOps vs. GitHub Runners Comparison

| Aspect | Azure DevOps Agents | GitHub Runners |
|--------|-------------------|----------------|
| **Hosted** | Microsoft-hosted & self-hosted | GitHub-hosted & self-hosted |
| **Cloud VMs** | ubuntu-latest, windows-latest | ubuntu-latest, windows-latest, macos-latest |
| **Terminology** | Agent, Agent Pool | Runner, Runner Group |
| **Configuration** | agent.conf, capabilities | runner config, labels |
| **Scaling** | VM Scale Sets, manual | Action workflows, GitHub-managed |
| **Cost Model** | Concurrent pipelines, minutes | Actions minutes, IP allowlisting |
| **Integration** | Azure Repos, GitHub, GitLab | GitHub native, supports external |

---

## Quick Reference: CLI Commands

### Azure DevOps Agent (Self-Hosted)

```bash
# List all agents in a pool
az pipelines agent list --pool-id 1 --organization https://dev.azure.com/yourorg

# Get agent status
az pipelines agent show --agent-id 5 --pool-id 1

# Tag/Update agent capabilities
az pipelines agent show --agent-id 5 --pool-id 1 --include-capabilities
```

### GitHub Runners

```bash
# List all self-hosted runners
gh api repos/{owner}/{repo}/actions/runners

# Create a new runner
gh auth token | gh api -H 'Accept: application/vnd.github.v3+json' repos/{owner}/{repo}/actions/runners/registration-token
```

---

## Summary

Mastering agent concepts is critical for DevOps roles:

- **Understand the types**: Hosted vs. self-hosted trade-offs
- **Master pool strategies**: Shared vs. dedicated pools
- **Know operations**: Scaling, patching, monitoring
- **Security first**: Token rotation, access control
- **Optimize always**: Queue time, caching, autoscaling

Be ready to discuss your experience managing agents, scaling them under load, and troubleshooting agent-related issues in interviews.
