# CI/CD & GitOps – STAR Stories

## Story – Improving CI/CD reliability and reducing flaky failures

**Situation**  
A critical release pipeline failed intermittently during integration tests and Kubernetes deployment stages, creating uncertainty around releases and increasing manual intervention.

**Task**  
Increase CI/CD reliability, make failures explainable, and reduce the operational overhead during releases.

**Action**

- Introduced **structured logging and metrics** in pipelines to capture stage duration, failure reasons, and test flakiness.
- Split unreliable end-to-end tests into a quarantined suite and added controlled **retries with backoff**, while keeping core sanity tests strict.
- Implemented **pre-flight checks** for Kubernetes deployments: cluster health, node readiness, image availability, Helm lint, and configuration validation.
- Standardized deployment templates and used GitOps/ArgoCD where possible to ensure **declarative, repeatable rollouts**.
- Connected CI/CD logs to observability tools so application metrics, logs, and pipeline runs could be correlated.

**Result**  
Increased pipeline success rate from roughly 80% to over 95% and significantly reduced mean time to recovery for failed runs.
