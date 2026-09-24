# Topic-10: Terraform Production Best Practices

## Overview

Terraform is widely used to provision and manage cloud infrastructure. In production, the challenge is not only creating resources, but also controlling how infrastructure changes are reviewed, applied, secured, and recovered.

A production-ready Terraform setup should provide:

- Consistent and repeatable infrastructure changes.
- Secure and reliable state management.
- Clear separation of environments and infrastructure ownership.
- Controlled deployments through CI/CD.
- Visibility into proposed changes and infrastructure drift.
- A documented recovery approach.

## 1. Use a Remote Backend

Terraform state keeps track of the relationship between the configuration and the real infrastructure.

For team environments, storing state locally can create collaboration and reliability challenges. A remote backend allows authorized team members and automation to access shared state.

For example, an AWS S3 backend can be configured as follows:

```hcl
terraform {
  backend "s3" {
    bucket       = "company-terraform-state"
    key          = "networking/prod/terraform.tfstate"
    region       = "us-east-1"
    use_lockfile = true
    encrypt      = true
  }
}
```

Before using this configuration, create the bucket and configure the required access permissions. Confirm that the chosen Terraform and AWS provider versions support the backend options being used.

### Key considerations

- Restrict access to the state bucket.
- Enable encryption and appropriate state protection.
- Use state locking to prevent conflicting operations.
- Keep state files out of source control.
- Establish a backup and recovery approach.

State files may contain sensitive information, so access should be limited even when outputs are marked as sensitive.

## 2. Design Clear State Boundaries

Avoid managing every resource in one large Terraform state.

Organize state around meaningful boundaries such as infrastructure ownership, lifecycle, and operational risk.

Example:

```text
Terraform
|
|-- networking
|   |-- VPC
|   |-- Subnets
|   |-- Route Tables
|
|-- application
|   |-- Compute
|   |-- Load Balancer
|
|-- data
    |-- Database
    |-- Storage
```

This is an illustrative structure; the right boundaries depend on how infrastructure is owned and operated.

### Why state boundaries matter

- A change to one component is less likely to affect unrelated infrastructure.
- Teams can manage separate components with clearer ownership.
- Plans and applies can be scoped to a smaller set of resources.
- State access can be controlled according to operational responsibilities.

Avoid splitting state into too many tiny units without a clear reason. Excessive separation can increase dependency and coordination overhead.

## 3. Use Reusable Modules

Terraform modules help standardize common infrastructure patterns and reduce repeated configuration.

For example, an organization may maintain reusable modules for:

- VPC and subnet creation.
- Security groups.
- Compute resources.
- Application load balancers.
- Monitoring and logging components.

A module should have a clear purpose, documented inputs and outputs, and predictable behavior.

### Example module structure

```text
terraform-project/
|
|-- main.tf
|-- variables.tf
|-- outputs.tf
|
|-- modules/
    |
    |-- vpc/
    |   |-- main.tf
    |   |-- variables.tf
    |   |-- outputs.tf
    |
    |-- security-group/
        |-- main.tf
        |-- variables.tf
        |-- outputs.tf
```

### Module practices

- Keep modules focused on a well-defined responsibility.
- Define and validate required inputs.
- Use outputs to expose only the values callers need.
- Document how to use and configure the module.
- Version shared modules and review changes before adopting them.
- Test modules before using them in critical environments.

Avoid creating overly generic modules that hide important infrastructure behavior or become difficult to maintain.

## 4. Separate Environments Deliberately

Development, testing, and production environments may have different requirements for access, availability, scale, and risk.

Choose an environment structure that supports those differences.

One possible approach is to keep separate configurations for each environment while reusing common modules:

```text
environments/
|
|-- dev/
|   |-- main.tf
|   |-- backend.tf
|   |-- dev.tfvars
|
|-- test/
|   |-- main.tf
|   |-- backend.tf
|   |-- test.tfvars
|
|-- prod/
    |-- main.tf
    |-- backend.tf
    |-- prod.tfvars

modules/
|
|-- vpc/
|-- application/
|-- database/
```

This approach can make environment-specific settings, backends, and permissions easier to review independently.

Terraform CLI workspaces can also maintain separate state for multiple instances of the same configuration. However, workspaces do not inherently provide separate credentials or access controls.

### Choose based on isolation needs

Consider separate configurations and backends when environments require:

- Different credentials or permissions.
- Stronger production/non-production boundaries.
- Independent release schedules or ownership.
- Different infrastructure lifecycles.

Use workspaces when the configuration and access model are sufficiently shared and the workspace separation fits the operational requirements.

## 5. Use a Controlled CI/CD Workflow

Infrastructure changes should be visible and reviewed before they are applied.

A typical Terraform CI/CD workflow:

```text
Developer opens a pull request
              |
              v
      Format and validate
              |
              v
       Generate a plan
              |
              v
       Review the changes
              |
              v
       Approve the change
              |
              v
        Apply the plan
              |
              v
     Verify the deployment
```

### Common validation commands

Format Terraform configuration:

```bash
terraform fmt -check
```

Validate the configuration:

```bash
terraform validate
```

Generate a plan:

```bash
terraform plan
```

### Production workflow considerations

- Run formatting and validation checks automatically.
- Generate plans for review before applying.
- Protect production apply steps with appropriate approval controls.
- Use a controlled identity for automation.
- Keep deployment logs and records of changes.
- Ensure the plan being applied is the one that was reviewed.

Avoid running unreviewed infrastructure changes directly from a developer's workstation against production.

## 6. Protect Secrets and Credentials

Do not hardcode passwords, access keys, tokens, or other secrets in Terraform configuration or committed variable files.

Use an approved secrets-management solution and secure identity mechanisms for CI/CD.

### Important considerations

- Avoid committing secret values to Git.
- Restrict access to Terraform state because it may contain sensitive values.
- Avoid printing secrets in logs or outputs.
- Prefer short-lived credentials or role-based access for automation where supported.
- Mark sensitive variables and outputs appropriately, while remembering that this does not remove their values from state.

Example of a sensitive input variable:

```hcl
variable "database_password" {
  type      = string
  sensitive = true
}
```

The `sensitive` setting helps prevent accidental display in certain Terraform output contexts. It is not a substitute for secure storage, state protection, or access controls.

## 7. Pin Provider and Module Versions

Version constraints help make Terraform behavior more predictable across development and deployment environments.

Example:

```hcl
terraform {
  required_version = ">= 1.5.0"

  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}
```

Use a dependency lock file to record selected provider versions. Commit the lock file to version control so team members and automation use consistent provider selections.

### Version management practices

- Set an intentional Terraform version requirement.
- Define provider version constraints.
- Commit and review `.terraform.lock.hcl`.
- Review provider and module updates before adopting them.
- Test version upgrades in a non-production environment first.

Avoid unreviewed dependency upgrades in production workflows.

## 8. Apply Least-Privilege Access

Terraform automation should have only the permissions needed for its tasks.

Avoid using broad, long-lived credentials for routine infrastructure operations.

### Access practices

- Use dedicated identities for automation.
- Limit permissions to required resources and actions.
- Separate production access from non-production access when appropriate.
- Restrict who can approve and execute production changes.
- Review permissions periodically.
- Keep credentials out of source code and build logs.

The permissions required for a plan may differ from those required for an apply, depending on the resources and provider operations involved. Design roles around the actual workflow and test them before production use.

## 9. Review Plans and Manage Drift

A Terraform plan shows the changes Terraform proposes based on the configuration, state, and observed infrastructure.

Review the plan before applying, paying particular attention to:

- Resource replacements.
- Resource destruction.
- Changes to network access or security settings.
- Changes to critical data resources.
- Unexpected differences from the previous configuration.

### Infrastructure drift

Drift occurs when the real infrastructure changes outside the Terraform workflow, or when the configuration and deployed resources no longer match.

For example, a team member might manually change a cloud resource in the console. A later Terraform plan may identify that difference.

### Drift management practices

- Run plans periodically to identify unexpected changes.
- Investigate the cause of drift before applying a plan.
- Decide whether to update the configuration or restore the intended configuration in the infrastructure.
- Document any approved manual changes and their follow-up.
- Avoid automatically applying changes without understanding their impact.

Not every difference should be overwritten. First establish whether the observed change was intentional and whether the configuration should be updated.

## 10. Plan for Recovery

Production infrastructure management should include a recovery approach for both configuration and state.

### Recovery practices

- Keep Terraform configuration in version control.
- Protect remote state and use the backend's available backup or versioning capabilities.
- Document how to recover state and who is authorized to do so.
- Record infrastructure ownership and dependencies.
- Test recovery procedures in a non-production environment where possible.
- Keep operational runbooks available to the team.

Avoid manually editing state files. Use Terraform state commands and documented recovery procedures when state maintenance is required.

## Common Production Mistakes

### 1. Sharing State Without Proper Protection

Remote state supports collaboration, but it must be protected with appropriate access controls and recovery procedures.

### 2. Using One Large State for Everything

A single state for unrelated systems can increase the impact and complexity of changes. Define boundaries based on ownership, lifecycle, and operational needs.

### 3. Treating Workspaces as Full Environment Isolation

Workspaces separate state, but they do not automatically separate credentials, permissions, or infrastructure lifecycles.

### 4. Applying Without Reviewing the Plan

A plan may include replacements or deletions that have significant operational impact. Review and approve production changes before applying.

### 5. Storing Secrets in Configuration or Git

Hardcoded secrets can be exposed through repositories, logs, or state. Use secure secret storage and protect state access.

### 6. Allowing Uncontrolled Version Changes

Unreviewed provider or module upgrades can introduce unexpected behavior. Pin versions and test upgrades deliberately.

### 7. Ignoring Drift

Uninvestigated drift can lead to Terraform plans that make unexpected changes. Identify why the infrastructure differs before applying a correction.

## Production Readiness Checklist

- [ ] Remote state is configured with secure access.
- [ ] State locking is enabled where supported.
- [ ] State boundaries reflect infrastructure ownership and lifecycle.
- [ ] Reusable modules are focused, documented, and versioned.
- [ ] Environment separation matches access and operational needs.
- [ ] CI/CD runs formatting, validation, and plan checks.
- [ ] Production changes require review and approval.
- [ ] The reviewed plan is the one applied.
- [ ] Secrets are not hardcoded or committed.
- [ ] State access is restricted and sensitive data is protected.
- [ ] Provider and module versions are controlled.
- [ ] Automation uses least-privilege permissions.
- [ ] Drift is checked and investigated.
- [ ] Recovery procedures are documented and tested.

## Key Takeaway

Production Terraform is a system for managing infrastructure change, not just a collection of resource definitions.

Reliable operations come from combining clear state boundaries, secure access, reusable configuration, controlled delivery, and a recovery plan.

**Architect-level insight:** The goal is not simply to automate infrastructure creation. It is to make infrastructure changes predictable, reviewable, secure, and recoverable throughout the system's lifecycle.

<img width="1024" height="1536" alt="Terraform production best practices" src="https://github.com/user-attachments/assets/8acc84c7-fdae-4231-bc38-0967bcc1f1ce" />
