# Ansible Interview Guide

## Overview

Ansible is a configuration management and automation tool used for provisioning, deploying, and managing infrastructure as code. This guide provides interview-ready answers about Ansible experience.

---

## Core Ansible Pitch (1-2 sentences)

"Most of my Ansible work has been around automating server provisioning and configuration for Linux fleets, standardizing app deployments, and integrating those playbooks into CI/CD so environments are reproducible and compliant."

---

## Interview Answer: "What have you done with Ansible?"

### Concise Answer (90 seconds)

"In recent roles, Ansible was the main **configuration** automation tool for our Linux servers. I created reusable roles for OS hardening, user/SSH management, and app stack setup (Nginx, Java services, systemd), and wired them to inventories so the same playbooks worked across dev, QA, and prod via different group_vars. We triggered these playbooks from our CI/CD (GitHub Actions/Azure DevOps), so provisioning and configuration were fully automated, idempotent, and auditable instead of manual runbooks."

---

## Concrete Examples

### 1. Reusable Roles Created

#### User and SSH Hardening
- Configured sudo access with least privilege
- Hardened sshd_config (disabled root login, password auth)
- Managed authorized_keys for team members
- Set password policies and account expiration

#### Application Stack Configuration
- Installed and configured Nginx/Apache with custom configs
- Deployed Java/Node.js applications
- Configured applications as systemd units for auto-restart
- Managed application configuration files via templates

#### System Configuration
- OS package management and updates
- Firewall rules (iptables/firewalld)
- Logging and monitoring agent installation
- Timezone, NTP, and DNS configuration

### 2. Inventory Management

**Parameterized inventories and group_vars:**
- Organized hosts by environment (dev/qa/prod)
- Used group_vars for environment-specific configurations
- Minimal overrides needed between environments
- Same playbooks worked across all environments

### 3. Ansible Use Cases

#### Infrastructure Bootstrapping
- Provisioned new EC2/VM instances
- Attached instances to load balancers
- Registered instances in monitoring systems (Prometheus, Datadog)
- Configured backup agents and security tools

#### Application Deployment
- Ran idempotent deployments during releases
- Updated application configurations
- Performed rolling restarts with health checks
- Cleared caches and ran migration scripts

#### CI/CD Integration
- Playbooks triggered from GitHub Actions/Azure DevOps pipelines
- Infrastructure and config changes ran as code-reviewed jobs
- All executions logged centrally for audit
- Easy rollback by re-running previous playbook version

---

## Sample Ansible Code

### Example Playbook Structure

```yaml
# site.yml - Main playbook
---
- name: Configure Web Servers
  hosts: webservers
  become: yes
  roles:
    - common
    - security-hardening
    - nginx
    - application

- name: Configure Database Servers
  hosts: dbservers
  become: yes
  roles:
    - common
    - security-hardening
    - postgresql
```

### Example Role: SSH Hardening

```yaml
# roles/security-hardening/tasks/main.yml
---
- name: Configure SSH daemon
  template:
    src: sshd_config.j2
    dest: /etc/ssh/sshd_config
    owner: root
    group: root
    mode: '0600'
  notify: restart sshd

- name: Disable root login
  lineinfile:
    path: /etc/ssh/sshd_config
    regexp: '^PermitRootLogin'
    line: 'PermitRootLogin no'
  notify: restart sshd

- name: Configure sudo access
  template:
    src: sudoers.j2
    dest: /etc/sudoers.d/team
    mode: '0440'
    validate: 'visudo -cf %s'
```

### Example Role: Application Deployment

```yaml
# roles/application/tasks/main.yml
---
- name: Create application user
  user:
    name: appuser
    system: yes
    shell: /bin/bash

- name: Deploy application binary
  copy:
    src: "{{ app_binary }}"
    dest: /opt/app/app
    owner: appuser
    group: appuser
    mode: '0755'
  notify: restart application

- name: Configure application
  template:
    src: app.conf.j2
    dest: /opt/app/config/app.conf
    owner: appuser
    group: appuser
  notify: restart application

- name: Install systemd service
  template:
    src: app.service.j2
    dest: /etc/systemd/system/app.service
  notify:
    - reload systemd
    - restart application

- name: Enable and start application
  systemd:
    name: app
    enabled: yes
    state: started
```

### Example Inventory Structure

```ini
# inventory/production/hosts
[webservers]
web1.example.com
web2.example.com
web3.example.com

[dbservers]
db1.example.com
db2.example.com

[production:children]
webservers
dbservers
```

```yaml
# inventory/production/group_vars/all.yml
---
env: production
log_level: info
ntp_servers:
  - 0.pool.ntp.org
  - 1.pool.ntp.org

# inventory/production/group_vars/webservers.yml
---
nginx_worker_processes: 4
app_version: "1.2.3"
app_port: 8080
```

### CI/CD Integration Example

```yaml
# .github/workflows/deploy.yml
name: Deploy Application

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Install Ansible
        run: |
          pip install ansible
          
      - name: Run Ansible playbook
        env:
          ANSIBLE_HOST_KEY_CHECKING: False
        run: |
          ansible-playbook -i inventory/production site.yml \
            --extra-vars "app_version=${{ github.sha }}" \
            --vault-password-file <(echo "${{ secrets.ANSIBLE_VAULT_PASSWORD }}")
```

---

## Key Benefits Achieved

### Automation Benefits
- **Eliminated manual server configuration** - Reduced human error
- **Idempotent operations** - Safe to run repeatedly
- **Version controlled** - All changes tracked in Git
- **Auditable** - Complete history of infrastructure changes
- **Reproducible environments** - Dev matches prod configuration

### Operational Benefits
- **Faster provisioning** - New servers configured in minutes vs hours
- **Consistent configuration** - No configuration drift
- **Easy rollback** - Revert to previous playbook version
- **Centralized management** - Single source of truth for configuration
- **Compliance** - Automated enforcement of security standards

---

## Follow-up Questions and Answers

### Q: "How did you handle secrets in Ansible?"

**Answer:**
"I used Ansible Vault to encrypt sensitive variables like passwords and API keys. Secrets were stored in encrypted vault files, and the vault password was managed through CI/CD secrets or a password manager. For production systems, we also integrated with HashiCorp Vault to fetch secrets at runtime rather than storing them in playbooks."

### Q: "How did you test Ansible playbooks?"

**Answer:**
"I used Molecule for testing Ansible roles - it would spin up Docker containers or VMs, run the role, and verify the expected state. We also had syntax checks (`ansible-playbook --syntax-check`) and dry-runs (`--check` mode) in CI before applying to real infrastructure. Critical playbooks were tested in dev/QA environments before production."

### Q: "How did you manage Ansible at scale?"

**Answer:**
"We used dynamic inventories that pulled host information from cloud providers (AWS EC2, Azure) or our CMDB. This kept inventory automatically up-to-date. We also used Ansible Tower (AWX) for larger deployments to provide RBAC, job scheduling, and centralized logging of playbook runs."

### Q: "What challenges did you face with Ansible?"

**Answer:**
"One challenge was managing playbook execution time for large inventories - we addressed this with parallelism tuning (`forks` setting) and by breaking large playbooks into smaller, targeted ones. Another was handling differences between RHEL and Ubuntu systems - we used conditional tasks and separate variable files per OS family to handle this."

---

## When to Mention Ansible in Interviews

### Good contexts:
- Configuration management discussions
- Infrastructure automation questions
- CI/CD pipeline integration
- Server provisioning and bootstrapping
- Compliance and security hardening
- Day-2 operations (patches, updates, config changes)

### Example bridges:
- "For configuration management, I used Ansible because..."
- "We automated server provisioning with Ansible playbooks that..."
- "To ensure consistency across environments, I created Ansible roles that..."

---

## Related Technologies to Mention

### Complementary Tools
- **Terraform** - Infrastructure provisioning (Terraform creates infra, Ansible configures it)
- **Packer** - Building golden images with Ansible provisioners
- **Jenkins/GitHub Actions** - CI/CD platforms that trigger Ansible playbooks
- **Vault** - Secret management integrated with Ansible
- **AWX/Tower** - Enterprise Ansible management platform

### Alternative Tools (show awareness)
- **Chef** - Ruby-based configuration management
- **Puppet** - Agent-based configuration management
- **SaltStack** - Event-driven automation platform

---

## Practice Talking Points

1. **Lead with the business value**: "Ansible automated our server configuration, reducing provisioning time from days to hours."

2. **Show technical depth**: Mention specific modules (apt, yum, systemd, template, lineinfile)

3. **Emphasize best practices**: Idempotency, roles, handlers, templates, vault

4. **Connect to CI/CD**: "Integrated Ansible into our pipelines for automated, auditable deployments"

5. **Highlight scale**: "Managed 200+ servers across multiple environments using dynamic inventories"

---

## Summary Statement

"I've used Ansible extensively for configuration management and automation - creating reusable roles for OS hardening and application deployment, integrating playbooks into CI/CD pipelines, and managing infrastructure across dev, QA, and production environments. The key benefit was moving from manual, error-prone server configuration to automated, idempotent, and auditable infrastructure-as-code."
