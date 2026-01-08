# Azure DevOps Agent Pools – STAR Stories

## Story 1 – Stabilizing flaky self-hosted agents

**Situation**  
In a large healthcare project, self-hosted Linux and Windows agents in shared Azure DevOps agent pools were failing mid-pipeline, causing frequent reruns and delaying releases for multiple product teams.

**Task**  
Own the reliability and capacity planning of the agent pools and reduce pipeline failures and wait times.

**Action**

- Split a single shared pool into **dedicated pools per team/environment** and tagged agents by capability (Docker, kubectl, .NET SDK, Node, security scanners).
- Backed the pools with **auto-scaling VM scale sets**, scaling agents based on queued jobs and historical concurrency.
- Implemented health checks and **drain logic** so unhealthy agents were removed from the pool, patched, and re-imaged without impacting running jobs.
- Standardized base images using Packer/Terraform so agents were **immutable and consistent**, eliminating configuration drift.

**Result**  
Reduced agent-related pipeline failures by ~60% and cut average queue time from several minutes to under a minute during peak hours, improving developer experience and release predictability.

---

## Story 2 – Optimizing agent pool cost and utilization

**Situation**  
Agent infrastructure costs were rising because agent VMs stayed online 24/7, even during off-peak hours, and pools were overprovisioned for typical load.

**Task**  
Optimize agent pool utilization and reduce cost without compromising SLAs for build wait times.

**Action**

- Analyzed Azure DevOps pipeline metrics to understand peak concurrency and typical workloads.
- Right-sized **VM SKUs and pool sizes** based on real usage instead of worst-case assumptions.
- Implemented **off-peak scale-in policies** and schedules to shut down or scale in idle agents during nights and weekends.
- Added monitoring dashboards and monthly reviews for agent utilization, failures, and queue times.

**Result**  
Lowered agent infrastructure cost by roughly 25–30% while keeping build wait times within agreed SLAs for development and release pipelines.
