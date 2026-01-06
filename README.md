# DevOps Engineer Interview Preparation

> **🎯 Interview Focus Update**: Based on panel member feedback, this role is **purely around DevOps Platform Administration**. Key focus areas for the first round include:
> - **Azure DevOps**: Linux server agents, Windows agents, agent pools administration
> - **GitHub**: Runners and GitHub administration
> - **SonarQube**: Code quality and integration
> - **JFrog Artifactory**: Artifact management
> - **GitOps with ArgoCD**: Kubernetes deployments

## Interview Details

- **Duration**: 45 minutes (may run longer)
- **Format**: Video interview
- **Dress Code**: Business professional
- **Method**: STAR (Situation, Task, Action, Result)

## Role Overview

### Position Summary
**DevOps Platform Administrator** supporting internal DevOps platform capabilities within a regulated healthcare environment. Primary focus on administering Azure DevOps agents (Linux/Windows), GitHub runners, JFrog Artifactory, SonarQube, and implementing GitOps with ArgoCD for Kubernetes deployments across Azure and GCP.
### Key Responsibilities
- Design, implement, and maintain CI/CD pipelines (Azure DevOps, ArgoCD, GitOps)
- Support cloud platform infrastructure across Azure and GCP in hybrid/regulated environments
- Administer Azure DevOps agent pools (self-hosted Linux and Windows agents)
- Manage GitHub runners and GitHub organization administration
- Configure and maintain JFrog Artifactory for artifact management
- Integrate SonarQube for code quality gates in CI/CD pipelines
- Deploy, operate, and support containerized workloads on Kubernetes (AKS/GKE)
- Drive platform-wide initiatives including GitHub → Azure DevOps migrations
- Implement artifact management and build promotion workflows (JFrog Artifactory)
- Enhance observability across CI/CD pipelines, Kubernetes platforms, and tooling
- Develop automation scripts primarily in PowerShell
- Partner with application, platform, and infrastructure teams
- Participate in on-call rotations providing L3-level support
- Operate within healthcare compliance requirements (HIPAA, SOC2)

### Required Technical Skills
- **Cloud Platforms**: Azure and GCP (hybrid enterprise environments)
- **Containers & Orchestration**: Kubernetes (AKS, GKE), Docker
- **CI/CD & GitOps**: Azure DevOps, GitHub, ArgoCD, GitOps workflows
- **Artifact Management**: JFrog Artifactory (preferred), Nexus, GitLab
- **Scripting & Automation**: PowerShell (required); Bash or Python a plus
- **Observability**: CI/CD and platform monitoring, logging, operational tooling
- **Source Control**: Git-based workflows, build agent configuration

## Interview Preparation Structure

### Core Question Categories

1. **"Tell Me About Yourself"**
2. **CI/CD & GitOps** - Azure DevOps, GitHub, ArgoCD stories
3. **Kubernetes & Platform Ownership** - AKS/GKE, production incidents
4. **Cloud Platforms** - Azure/GCP hybrid environments
5. **Migrations** - GitHub → Azure DevOps, source control transitions
6. **Artifact Management** - JFrog/Nexus/GitLab workflows
7. **PowerShell & Automation** - Platform onboarding, operational scripts
8. **Observability & On-call** - Monitoring, L3 support, incident response
9. **Compliance & Security** - Regulated environments, HIPAA/SOC2
10. **Behavioral** - Team collaboration, conflict, decision-making

## Repository Structure

.
├── README.md                          # This file
├── STAR-STORIES.md         # Introduction script
├── star-stories/
│   ├── ci-cd-gitops.md                # CI/CD and GitOps stories
│   ├── kubernetes-platform.md         # Kubernetes and platform stories
│   ├── cloud-azure.md                 # Cloud platform stories
│   ├── migrations.md                  # Migration stories
│   ├── artifact-management.md         # Artifact management stories
│   ├── powershell-automation.md       # PowerShell automation stories
│   ├── observability-oncall.md        # Observability and on-call stories
│   ├── compliance-security.md         # Compliance and security stories
│   └── behavioral.md                  # Behavioral stories
└── cheat-sheet.md                     # Quick reference bullet points


### During the Interview
- **Always explain** - Never answer with just "yes" or "no"
- **Use STAR format** for every question
- **Time your answers** - Aim for 1.5-2 minutes per story
- **Ask clarifying questions** if needed
- **Show enthusiasm** for platform engineering and DevOps
- **Mention specific tools** from the job requirements

### Questions to Ask Them
1. "What does success look like for this role in the first 90 days?"
2. "How does the team measure platform reliability and developer experience?"
3. "What's the team's approach to on-call and incident management?"
4. "What are the biggest platform challenges you're facing right now?"
5. "How does the team balance new feature work with operational excellence?"

## Quick Links

- [Full STAR Stories](./STAR-STORIES.md)
- - [Tell Me About Yourself Script](./STAR-STORIES.md)
- [Interview Cheat Sheet](./cheat-sheet.md)
