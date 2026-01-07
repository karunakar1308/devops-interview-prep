# 1-Week Cloud Security Architect Study Guide - ODJFS

## 🔥 CRITICAL: Your Daily Plan (7 Days)

You have **1 week**. This is a focused, no-fluff study plan.

### Day 1 (Today): Core Foundations
- [ ] Read this entire file (2 hours)
- [ ] Read README.md in this folder
- [ ] Read 30-60-90 day plan
- [ ] Practice your 60-second pitch 10 times
- [ ] Review Microsoft Defender for Cloud basics (see below)

### Day 2: Technical Deep Dive Part 1
- [ ] Microsoft Defender for Cloud - study implementation approach
- [ ] Zero Trust principles - map to your Apple EKS work
- [ ] AWS/Azure security services comparison
- [ ] STRIDE framework - practice applying to 2 scenarios

### Day 3: Technical Deep Dive Part 2  
- [ ] MITRE ATT&CK for Cloud - know 10 key tactics
- [ ] Policy-as-code - connect OPA/Gatekeeper to Azure Policy
- [ ] IAM/RBAC - privileged access patterns
- [ ] Compliance frameworks (NIST, CIS, ISO, SOC 2, HIPAA)

### Day 4: DevSecOps & Kubernetes Security
- [ ] DevSecOps pipeline security - map to your experience
- [ ] Kubernetes security - RBAC, network policies, PSS
- [ ] IaC security scanning - Terraform, CloudFormation
- [ ] Container security - image scanning, hardening

### Day 5: ODJFS-Specific Prep
- [ ] Review 30-60-90 day plan again
- [ ] Prepare 5 architecture scenarios (see below)
- [ ] Adapt STAR stories for security architecture focus
- [ ] Research ODJFS (website, news, projects)

### Day 6: Practice & Mock Interview
- [ ] Mock interview - have someone ask you questions
- [ ] Practice whiteboarding secure architecture
- [ ] Practice explaining Defender implementation
- [ ] Review all your talking points

### Day 7 (Day Before Interview): Final Review
- [ ] Read this file again
- [ ] Review 30-60-90 day plan
- [ ] Practice 60-second pitch 5 times
- [ ] Review your Apple/Fiserv/JPMC security stories
- [ ] Get good sleep

---

## 🎯 Part 1: Microsoft Defender for Cloud (JD Priority #1)

### What It Is
Microsoft Defender for Cloud is a **Cloud Security Posture Management (CSPM)** and **Cloud Workload Protection Platform (CWPP)** that:
- Continuously assesses security posture across Azure, AWS, GCP, and on-prem
- Provides security recommendations based on benchmarks (CIS, NIST)
- Detects threats and vulnerabilities in workloads
- Enforces regulatory compliance (HIPAA, SOC 2, etc.)

### Key Components

**1. Secure Score**
- Dashboard showing overall security posture (0-100%)
- Prioritized recommendations to improve score
- Tracks improvement over time

**2. Regulatory Compliance Dashboard**
- Shows compliance with NIST 800-53, CIS, ISO 27001, HIPAA, etc.
- Maps controls to your resources
- Generates compliance reports for auditors

**3. Workload Protections**
- Defender for Servers (VMs)
- Defender for Containers (AKS, EKS, GKE)
- Defender for Storage
- Defender for Databases
- Defender for Key Vault

**4. Defender for Identity**
- Monitors on-prem Active Directory
- Detects lateral movement, pass-the-hash, golden ticket attacks
- Integrates with cloud identity (Entra ID)

### How You'll Implement It at ODJFS (Interview Answer)

**Phase 1 (First 30 days): Pilot**
> "I'd start with a pilot in a non-prod Azure subscription. Enable Defender for Cloud free tier to baseline security posture, then enable paid Defender plans for servers and containers. Assess current secure score, review top recommendations, and identify quick wins."

**Phase 2 (Days 31-60): Production Rollout**
> "Expand to production in phases - start with non-critical workloads. Configure Defender for Servers, Containers, Storage. Integrate alerts with existing SIEM/ticketing. Set up regulatory compliance tracking for NIST and HIPAA. Train DAS/JFS teams on using recommendations."

**Phase 3 (Days 61-90): Automation & Multi-Cloud**
> "Deploy Defender for AWS and GCP using Arc-enabled servers. Implement automated remediation for common findings using Azure Policy. Create custom security policies for ODJFS-specific requirements. Establish metrics dashboard for leadership reporting."

### Key Features to Mention
- **Just-in-Time VM Access**: Reduce attack surface by opening RDP/SSH only when needed
- **Adaptive Application Controls**: Whitelist applications on VMs
- **File Integrity Monitoring**: Detect unauthorized file changes
- **Threat Intelligence**: Microsoft's global threat data

**Connect to Your Experience:**
> "At Apple, I implemented similar security posture management using OPA/Gatekeeper for Kubernetes compliance. Defender for Cloud extends this to the full cloud stack. At JPMC, I worked with enterprise security standards - Defender's regulatory compliance dashboard automates that alignment."

---

## 🔑 Part 2: STRIDE & MITRE ATT&CK (Your Gap Areas)

These are frameworks you need to demonstrate knowledge of.

### STRIDE Threat Modeling

STRIDE is a framework for identifying threats:

| Threat | Description | Cloud Example | Mitigation |
|--------|-------------|---------------|------------|
| **S**poofing | Pretending to be someone else | Stolen AWS access keys | MFA, short-lived tokens, IAM policies |
| **T**ampering | Modifying data/code | Malicious container image | Image signing, admission controllers |
| **R**epudiation | Denying actions | No audit logs | CloudTrail, Azure Activity Log, immutable logs |
| **I**nformation Disclosure | Exposing data | Public S3 bucket | Encryption, access controls, DLP |
| **D**enial of Service | Making service unavailable | Resource exhaustion | Rate limiting, auto-scaling, DDoS protection |
| **E**levation of Privilege | Gaining unauthorized access | Over-permissive IAM role | Least privilege, regular access reviews |

**How to Use in Interview:**
> "When reviewing a new cloud architecture, I apply STRIDE. For example, for a web app on AWS, I'd assess: Spoofing risk with API authentication, Tampering via signed deployments, Repudiation through CloudTrail logging, Information Disclosure with encryption and S3 bucket policies, DoS with WAF and auto-scaling, and Elevation of Privilege through least-privilege IAM roles."

### MITRE ATT&CK for Cloud

10 Key Tactics to Know:

1. **Initial Access**: Compromised credentials, phishing, exploiting public-facing apps
2. **Execution**: Running malicious code in cloud workloads
3. **Persistence**: Creating backdoor IAM users, implanting in container images
4. **Privilege Escalation**: Exploiting misconfigured IAM policies
5. **Defense Evasion**: Disabling CloudTrail, deleting logs
6. **Credential Access**: Accessing secrets from metadata service, Key Vault
7. **Discovery**: Enumerating cloud resources, IAM policies
8. **Lateral Movement**: Moving between accounts/subscriptions
9. **Collection**: Exfiltrating data from storage accounts
10. **Impact**: Deleting resources, cryptomining

**How to Use in Interview:**
> "I use MITRE ATT&CK to inform security controls. For example, to defend against Credential Access tactics, I ensure metadata service is restricted (IMDSv2), secrets are in Key Vault with RBAC, and we have alerting on unusual secret access patterns."

---

## 🛡️ Part 3: Quick Reference Cards

### Zero Trust Principles (Your Apple Work)

1. **Verify Explicitly**: Always authenticate and authorize (MFA, RBAC)
2. **Least Privilege Access**: Grant minimum necessary (IAM policies, JIT)
3. **Assume Breach**: Segment, monitor, encrypt everything

**Your Experience:**
- Apple: Strict IAM/RBAC, namespace isolation, network policies
- Fiserv: IAM-based controls, Istio mTLS for zero-trust networking

### Compliance Frameworks (1-Sentence Each)

- **NIST 800-53**: Federal security controls (ODJFS likely uses this)
- **CIS Benchmarks**: Prescriptive security configurations
- **ISO 27001**: International information security management standard
- **SOC 2**: Trust service criteria for service organizations
- **HIPAA**: Health data protection (citizen data = sensitive)
- **GDPR**: EU data privacy (may apply to ODJFS data)

### AWS vs Azure Security Services

| Function | AWS | Azure |
|----------|-----|-------|
| Identity | IAM | Entra ID (Azure AD), RBAC |
| Network Security | Security Groups, NACLs | NSGs, ASGs |
| Encryption | KMS | Key Vault |
| Monitoring | CloudTrail, CloudWatch | Activity Log, Monitor |
| Posture Mgmt | Security Hub | Defender for Cloud |
| Secrets | Secrets Manager | Key Vault |
| Policy | Config, AWS Organizations | Azure Policy |

---

## 💬 Part 4: Interview Questions & Answers

### Q: "How would you implement Microsoft Defender for Cloud at ODJFS?"

A: See 30-60-90 day plan and Defender section above. Key: Pilot → Phased rollout → Multi-cloud expansion.

### Q: "How do you apply Zero Trust to cloud architectures?"

A: "I apply three principles: verify explicitly with MFA and strong authentication, use least-privilege IAM with JIT access, and assume breach by segmenting networks, encrypting data, and monitoring continuously. At Apple, I implemented this on EKS using strict RBAC, namespace isolation, and network policies."

### Q: "Tell me about a time you secured a cloud environment."

STAR from Apple:
- **S**: Apple EKS platforms needed security architecture
- **T**: Define secure-by-default standards and implement guardrails
- **A**: Implemented Zero Trust (IAM/RBAC, network policies), policy-as-code (OPA/Gatekeeper), DevSecOps integration
- **R**: Secure-by-default platform, compliance guardrails, security integrated in CI/CD

### Q: "How would you train the DAS and JFS security teams?"

A: "I'd create a tiered approach: 'Cloud Security 101' for broad awareness, hands-on workshops for Defender for Cloud, office hours for questions, and runbooks for common scenarios. I've done this at Apple when establishing security SOPs and training teams on platform security."

### Q: "What's your approach to DevSecOps integration?"

A: "Shift security left by integrating into CI/CD: SAST for code, SCA for dependencies, IaC scanning (Checkov/Snyk), container image scanning, and security gates. At Apple and Fiserv, I integrated security scanning into GitHub Actions, ArgoCD, and Azure DevOps pipelines."

---

## 🎨 Part 5: Architecture Scenarios (Practice These)

### Scenario 1: Secure 3-Tier Web App

**Question**: "Design a secure cloud architecture for a citizen-facing web application."

**Your Answer** (whiteboard this):
- Web tier: ALB/App Gateway with WAF, in public subnet
- App tier: Auto-scaling group in private subnet, security groups allow only from ALB
- Data tier: RDS/Azure SQL in private subnet, encrypted at rest (KMS/TDE)
- Identity: Entra ID SSO with MFA
- Monitoring: CloudTrail/Activity Log, Defender for Cloud
- Network: VPC/VNet with private subnets, no direct internet access to app/data tiers
- Encryption: TLS for in-transit, KMS/Key Vault for at-rest
- Access: Bastion host or Just-in-Time VM access for admin

### Scenario 2: Container Security

**Question**: "How would you secure a Kubernetes deployment?"

**Your Answer**:
- **RBAC**: Least-privilege service accounts, namespace isolation
- **Network Policies**: Restrict pod-to-pod communication
- **Pod Security Standards**: Enforce restricted profile (no root, read-only filesystem)
- **Image Security**: Scan images, use trusted registries, sign images
- **Secrets**: Use external secrets operator with Key Vault
- **Monitoring**: Defender for Containers, Falco for runtime security
- **Policy**: OPA/Gatekeeper for admission control

This is what I did at Apple on EKS.

---

## ⏰ Day-Before-Interview Checklist

- [ ] Review this file one more time
- [ ] Review 30-60-90 day plan
- [ ] Practice 60-second pitch
- [ ] Review your Apple/Fiserv/JPMC security stories
- [ ] Prepare 3 questions to ask them:
  - "What's the current cloud security posture and biggest gaps?"
  - "What does success look like for this role in 90 days?"
  - "How does the team balance security with developer velocity?"
- [ ] Dress code: Business professional
- [ ] Bring notebook, pen
- [ ] Get good sleep!

---

## 🚀 Final Notes

**You've got this!** You have:
- 10 years cloud experience
- Security architecture experience at Apple
- Hybrid cloud (AWS/Azure) at Fiserv
- Enterprise security at JPMC
- All the required skills

This role is about **program establishment** - creating the Cloud Security Architecture function. You've done this before. Frame everything through that lens.

**Your advantage**: You're not just a security person who learned cloud, or a cloud person who learned security. You're a Cloud Architect who has embedded security throughout your career. That's rare and valuable.

Good luck! 🎉
