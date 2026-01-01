# Interview Cheat Sheet

> **Quick reference for last-minute review before your Humana DevOps Engineer interview**

## Your Elevator Pitch (30 seconds)

"I'm a DevOps Platform Engineer with 9 years of experience specializing in CI/CD automation, Kubernetes operations, and platform administration. I've managed Azure DevOps agent pools, GitHub runners, and implemented GitOps with ArgoCD at scale for companies like Apple and Fiserv. I excel at building reliable, secure platforms that enable development teams while maintaining compliance in regulated environments."

---

## Key Numbers to Remember

### Impact Metrics
- **35-45%** - Typical improvements in build times, costs, or efficiency
- **50%** - Deployment time reductions with GitOps
- **60%** - Agent wait time improvements
- **70%+** - Code coverage thresholds in SonarQube
- **40%** - Cost savings from GitHub runner optimization

### Scale
- **Hundreds** of build agents managed
- **8+ years** Kubernetes experience
- **Multiple** production Sev-1 incidents resolved
- **Enterprise scale** - Fortune 500 companies

---

## Platform Tools - Quick Facts

### Azure DevOps
- **Agent Types:** Self-hosted (Linux/Windows), Microsoft-hosted
- **Key Features:** Agent pools, capabilities, demands, YAML pipelines
- **Your Experience:** Configured pools, automated provisioning, monitored queue times
- **Pro Tip:** Always mention capability tags for routing specialized builds

### GitHub
- **Runner Types:** GitHub-hosted, self-hosted, ephemeral
- **Security:** Runner groups, scoped permissions, isolated subnets
- **Org Policies:** Branch protection, required reviews, secret scanning, Dependabot
- **Your Experience:** EC2/Azure VM runners, auto-scaling, 40% cost reduction

### ArgoCD (GitOps)
- **Core Concept:** Git as single source of truth
- **Sync Modes:** Auto-sync (non-prod), manual (prod)
- **Key Features:** Sync waves, health checks, ApplicationSets
- **Your Experience:** Eliminated drift, 50% faster deployments, simple Git rollbacks

### JFrog Artifactory
- **Repo Types:** Local, remote, virtual
- **Key Features:** Replication, retention policies, Xray scanning
- **Promotion:** Immutable artifacts promoted between repos (dev → test → prod)
- **Your Experience:** Centralized management, 30% faster retrieval, vulnerability scanning

### SonarQube
- **Quality Gates:** Coverage %, bugs, vulnerabilities, code smells
- **Strategy:** "New code" approach for legacy (strict on changes, backlog for existing debt)
- **Integration:** Pipeline stages, PR checks, fail builds on violations
- **Your Experience:** 50% increase in early bug detection, gradual adoption

### Kubernetes
- **Platforms:** AKS (Azure), GKE (GCP), EKS (AWS)
- **Key Concepts:** HPA, resource requests/limits, probes (readiness/liveness), RBAC
- **Observability:** Prometheus, Grafana, logging (ELK/Splunk)
- **Your Experience:** Production incidents, platform standardization, Helm charts

---

## STAR Formula Reminders

### Structure (1.5-2 minutes each)
1. **Situation** (15-20 sec) - Set context, company, problem
2. **Task** (10-15 sec) - Your specific responsibility
3. **Action** (45-60 sec) - What YOU did (use "I" not "we")
4. **Result** (20-30 sec) - Quantified outcomes, impact

### Power Phrases
- "I was responsible for..."
- "I implemented/designed/built..."
- "This resulted in [X% improvement]..."
- "The team/organization gained..."

---

## Technology Talking Points

### PowerShell Automation
- **Use Cases:** Agent provisioning, platform onboarding, scaffolding
- **Best Practices:** Parameterized, dry-run mode, versioned in repos
- **Integration:** Pipeline tasks, scheduled jobs, event-driven

### CI/CD Pipelines
- **Patterns:** Build → Test → Security Scan → Artifact Publish → Deploy
- **Security:** SAST/DAST, secret scanning, dependency scanning, approvals
- **Standards:** Reusable templates, shared libraries, consistent stages

### Compliance (HIPAA/SOC2)
- **Approach:** Automated guardrails, policy-as-code, audit trails
- **Balance:** Developer velocity with security controls
- **Evidence:** CI/CD logs, Git history, RBAC configs, dashboards

---

## Red Flags to Avoid

❌ **Don't say:**
- "We did this..." (use "I" to show your contribution)
- "It was easy" (minimizes your expertise)
- Just "yes" or "no" without explanation
- "I don't know" without offering to learn/discuss

✅ **Do say:**
- "I led the effort to..."
- "The challenge was [X], so I..."
- "Based on my experience, I would approach this by..."
- "That's not an area I've worked in, but here's how I'd approach it..."

---

## Common Follow-up Topics

### If they ask about...

**Failures/Mistakes:**
- Own it, describe what you learned
- Show how you improved processes afterward
- Example: "Early on, I didn't tune HPA properly, causing cost spikes. I learned to..."

**Teamwork/Conflict:**
- Focus on collaboration and resolution
- Show empathy and professionalism
- Example: "When security blocked our pipeline changes, I scheduled a working session to..."

**Technical Depth:**
- Be honest about limits
- Connect to related experience
- Offer thoughtful approach

**Why Humana:**
- Healthcare mission + technical challenge
- DevOps platform ownership aligns with your strengths
- Regulated environment experience (HIPAA similar to SOX/PCI)

---

## Last-Minute Reminders

### Body Language
- ✅ Smile and make eye contact
- ✅ Sit up straight, lean forward slightly (shows engagement)
- ✅ Use hand gestures naturally when explaining
- ✅ Nod when listening to show understanding

### Voice
- ✅ Speak clearly and at moderate pace
- ✅ Pause between STAR sections
- ✅ Vary tone to show enthusiasm
- ✅ It's OK to take 2-3 seconds to think before answering

### Technical Setup
- ✅ Close all other apps/tabs
- ✅ Silence phone and notifications
- ✅ Have water nearby
- ✅ Good lighting (face the window or use lamp)
- ✅ Camera at eye level

---

## Emergency Pivots

### If You Blank on a Question
"That's a great question. Let me think about a specific example... [pause 3 seconds]... At [Company], we had a situation where..."

### If You Don't Have That Exact Experience
"I haven't worked with [Tool X] specifically, but I have extensive experience with [Similar Tool Y], and the concepts are similar. For example..."

### If Interview Runs Short
"I'd love to hear more about [the team structure / recent platform wins / biggest current challenges]."

---

## Questions to Ask Them (Pick 2-3)

1. **Platform Health:** "How does the team currently measure platform reliability and developer satisfaction?"

2. **Current State:** "What's the current state of your GitOps adoption with ArgoCD? Are there any pain points?"

3. **Team Dynamics:** "How does the platform team collaborate with application teams? What's the feedback loop?"

4. **Growth:** "What would success look like in this role in the first 90 days? First 6 months?"

5. **Challenges:** "What are the biggest platform challenges or technical debt items the team is tackling?"

6. **On-call:** "Can you walk me through your on-call process and incident management approach?"

7. **Culture:** "How does the team balance new feature work with operational excellence and technical debt?"

8. **Technology:** "Are there any upcoming migrations or platform modernization initiatives planned?"

---

## Pre-Interview Mantra

> **"I have 9 years of relevant experience. I've solved real problems at scale. I'm prepared, qualified, and ready to contribute. I belong in this conversation."**

---

**Good luck! 🚀 You've got this!**
