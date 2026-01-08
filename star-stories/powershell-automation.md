# PowerShell Automation – STAR Stories

## Story – Automating Azure DevOps agent lifecycle with PowerShell, Ansible, and Terraform

**Situation**  
Self-hosted Azure DevOps agents were configured manually, leading to configuration drift, inconsistent toolchains, and security patch gaps across agents.

**Task**  
Automate the provisioning, configuration, and maintenance of build agents using PowerShell, Ansible, and Terraform.

**Action**

- Used **Terraform** to provision VM scale sets, networking, and IAM for agent pools, tagging resources by pool, OS, and capabilities.
- Created **Ansible** playbooks to install required tools (Docker, kubectl, Helm, .NET, Java, scanners) and to register agents to the correct Azure DevOps pools.
- Developed **PowerShell** scripts to:
  - Register and deregister agents from pools.
  - Perform automated patching and restarts with minimal disruption (drain, update, re-enable).
  - Collect diagnostics (logs, event viewer, agent logs) on demand for failed agents and attach them to incident tickets.
- Integrated these scripts into CI/CD so new agent images were **built, validated, and rolled out** with a repeatable process.

**Result**  
Reduced agent onboarding time from hours to under 15 minutes, eliminated most "works on one agent but not another" issues, and improved security posture by enforcing consistent, up-to-date images.
