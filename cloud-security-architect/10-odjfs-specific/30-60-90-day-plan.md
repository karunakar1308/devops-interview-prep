# 30-60-90 Day Plan - Cloud Security Architect at ODJFS

## Overview

This plan outlines how you would establish ODJFS's Cloud Security Architecture program over your first 90 days.

**Use this during your interview** to demonstrate strategic thinking and understanding of the role.

---

## First 30 Days: Discovery & Quick Wins

### Goals
- Understand current state
- Build relationships
- Identify quick security wins
- Begin Microsoft Defender for Cloud evaluation

### Week 1-2: Assessment & Stakeholder Mapping

**Activities:**
- Meet with IT Governance and Risk Management Office leadership
- Interview DAS and JFS security team members
- Document current cloud footprint (AWS, Azure, GCP usage)
- Inventory existing security tools and gaps
- Review current cloud security incidents and near-misses
- Understand compliance requirements (NIST, HIPAA, state regulations)
- Map out DevOps/DevSecOps team structure and current practices

**Deliverables:**
- Current state assessment document
- Stakeholder map (security, ops, dev, governance, audit)
- Initial risk register

### Week 3-4: Quick Wins & Tool Evaluation

**Activities:**
- Start Microsoft Defender for Cloud pilot in non-prod environment
- Identify and remediate "low-hanging fruit" security misconfigurations
- Document baseline security posture across cloud workloads
- Review existing IAM policies for overly permissive access
- Assess container and Kubernetes security posture
- Begin relationship building with application teams

**Quick Wins to Deliver:**
- Enable MFA enforcement for cloud admin accounts
- Implement CloudTrail/Azure Activity Log centralized logging
- Identify and tag unencrypted storage resources
- Document top 10 security findings with remediation roadmap

**Deliverables:**
- Defender for Cloud pilot report
- Quick wins remediation plan (with timelines)
- Initial security findings dashboard

---

## Days 31-60: Program Foundation & Standards

### Goals
- Establish Cloud Security Architecture program framework
- Define security standards and guardrails
- Expand Defender for Cloud deployment
- Begin DevSecOps integration

### Week 5-6: Program Structure & Standards

**Activities:**
- Define Cloud Security Architecture program charter
- Create cloud security reference architectures for common patterns:
  - Web application (3-tier)
  - API services
  - Data analytics workloads
  - Container/Kubernetes deployments
- Establish security review process for new cloud projects
- Draft cloud security policies (IAM, encryption, network, logging)
- Create security baseline configurations for AWS/Azure/GCP

**Deliverables:**
- Program charter document
- 3-5 reference architecture diagrams with security controls
- Security review intake form and process
- Cloud security policy v1.0 (draft for review)

### Week 7-8: Defender Rollout & DevSecOps Integration

**Activities:**
- Expand Microsoft Defender for Cloud to production (phased)
- Configure Defender for Identity for on-prem/hybrid AD monitoring
- Integrate Defender with existing SIEM/ticketing systems
- Work with DevOps teams to integrate security scanning in CI/CD:
  - Container image scanning
  - IaC security scanning (Terraform, CloudFormation)
  - Secret scanning
- Establish security metrics and KPIs
- Begin training materials development

**Deliverables:**
- Defender for Cloud production rollout plan
- DevSecOps security gate requirements document
- Security metrics dashboard (initial version)
- Training curriculum outline

---

## Days 61-90: Scale & Continuous Improvement

### Goals
- Roll out program organization-wide
- Train teams
- Establish operational rhythm
- Demonstrate measurable security improvements

### Week 9-10: Training & Enablement

**Activities:**
- Conduct "Cloud Security 101" training for DAS and JFS teams
- Run "Secure Cloud Architecture" workshop for application teams
- Create self-service security documentation (wiki/portal)
- Establish office hours for cloud security questions
- Train DevOps teams on security scanning tools
- Create runbooks for common security scenarios

**Deliverables:**
- Training materials and recordings
- Cloud security wiki/portal
- Security runbook library (10+ scenarios)
- Office hours schedule

### Week 11-12: Operationalize & Measure

**Activities:**
- Implement policy-as-code guardrails (Azure Policy, AWS Config, OPA)
- Establish regular security review cadence with project teams
- Configure automated security alerts and response workflows
- Complete Defender for Cloud full deployment
- Implement threat modeling process (STRIDE) for new architectures
- Conduct first quarterly security posture review with leadership
- Plan for next 90 days (advanced topics: zero trust, workload identity, etc.)

**Deliverables:**
- Policy-as-code implementation (enforcing key security controls)
- Automated alert and remediation workflows
- Quarterly security report (showing improvements)
- Cloud Security Architecture program roadmap (next 6 months)

---

## Success Metrics (What You'll Report at 90 Days)

### Quantitative Metrics
- **Security posture improvement**: X% reduction in critical/high findings
- **Defender for Cloud coverage**: 100% of production workloads onboarded
- **DevSecOps integration**: Security scans in X% of CI/CD pipelines
- **Mean time to remediation**: Reduced from X days to Y days
- **Policy violations**: X automated policy checks enforcing security
- **Training reach**: X team members trained on cloud security

### Qualitative Outcomes
- Cloud Security Architecture program established and operational
- Security review process integrated into project lifecycle
- Reference architectures adopted by application teams
- Strong relationships with security, ops, and dev teams
- Clear roadmap for next 6 months

---

## Key Relationships to Build

**Week 1 Priority:**
- IT Governance and Risk Management Office (your direct stakeholders)
- CISO or equivalent security leadership
- DAS (Department of Administrative Services) security team
- JFS security team
- Cloud platform team leads (AWS/Azure)

**Week 2-4:**
- DevOps/DevSecOps team leads
- Application development managers
- Compliance/audit team
- Network and infrastructure teams
- Key vendor contacts (Microsoft support for Defender)

---

## Risks & Mitigation

| Risk | Mitigation |
|------|------------|
| Resistance to new security processes | Emphasize enablement, not gatekeeping; show quick wins |
| Lack of cloud security skills in team | Prioritize training; bring in external resources if needed |
| Tool sprawl and integration challenges | Start with Defender for Cloud; consolidate tools over time |
| Shadow IT / ungoverned cloud usage | Discovery phase; educate vs punish; provide secure paths |
| Budget constraints for tools | Build business case with risk quantification; start small |

---

## Interview Tips: Using This Plan

**When they ask "What would you do in your first 90 days?"**

> "In my first 30 days, I'd focus on discovery and quick wins - understanding your current cloud posture, meeting stakeholders, and starting a Defender for Cloud pilot while delivering immediate security improvements like MFA enforcement and centralized logging.
>
> Days 31-60, I'd establish the program foundation - creating reference architectures, defining security standards, expanding Defender deployment, and integrating security into your DevSecOps workflows.
>
> Days 61-90, I'd focus on scaling through training, operationalizing the program with policy-as-code, and demonstrating measurable improvements. By day 90, you'd have a fully operational Cloud Security Architecture program with metrics showing security posture improvement, teams trained on tools, and a roadmap for continuous enhancement."

**Follow-up they might ask:**
- "What's your first priority?" → **Defender for Cloud evaluation and quick wins**
- "How would you handle resistance?" → **Emphasize enablement, show value fast, partner not police**
- "What if you don't have budget for tools?" → **Start with Defender (you're Microsoft-heavy), quantify risk, build business case**

---

## Your Relevant Experience to Mention

- **Apple**: Led cloud security architecture for EKS platforms - similar program establishment work
- **Fiserv**: Hybrid cloud security across AWS/Azure - matches ODJFS's multi-cloud environment
- **JPMC**: Enterprise security standards and governance - aligns with ODJFS's regulated environment
- **All roles**: Training teams, creating SOPs, integrating security into DevOps

**Frame it as**: "I've done similar program establishment work at Apple where I defined secure-by-default standards for our EKS platforms, created security SOPs, and trained teams. This role is about applying that same approach to ODJFS's broader cloud environment with Defender for Cloud as the cornerstone tool."
