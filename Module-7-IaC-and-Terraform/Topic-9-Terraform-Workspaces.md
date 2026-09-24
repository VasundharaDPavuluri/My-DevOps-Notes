# Topic-9: Terraform Workspaces

## Overview

Terraform workspaces allow us to manage multiple instances of the same configuration with separate state files.

They are useful when we need to create similar infrastructure for different environments or temporary testing without duplicating the entire configuration.

This note focuses on **Terraform CLI workspaces**. HCP Terraform workspaces use a different model.

## Why Do We Need Workspaces?

Consider a Terraform configuration that creates an EC2 instance.

We may need separate instances for development, testing, and production. Instead of maintaining separate copies of the same configuration, we can use workspaces to keep separate state for each instance.

Each workspace tracks its own resources. Switching workspaces changes which state Terraform uses.

## How Workspaces Work

Terraform starts with a workspace named `default`.

We can create additional workspaces, such as `dev`, `test`, and `prod`. The configuration remains the same, but each workspace has its own state.

```text
                 Terraform Configuration
                          |
          -------------------------------------
          |                 |                 |
         dev               test              prod
          |                 |                 |
      dev state         test state        prod state
          |                 |                 |
    Dev resources     Test resources    Prod resources
```

Terraform does not automatically create different settings for each workspace. Use input variables or variable files to provide environment-specific values.

## Common Workspace Commands

### 1. List Workspaces

List all workspaces in the current configuration:

```bash
terraform workspace list
```

The currently selected workspace is marked with an asterisk (`*`).

Example output:

```text
  default
* dev
  prod
  test
```

### 2. Create a Workspace

Create a new workspace:

```bash
terraform workspace new dev
```

Terraform creates the workspace and switches to it.

### 3. Select a Workspace

Switch to an existing workspace:

```bash
terraform workspace select dev
```

### 4. Show the Current Workspace

Check which workspace is currently selected:

```bash
terraform workspace show
```

### 5. Delete a Workspace

Delete a workspace that is no longer required:

```bash
terraform workspace delete dev
```

A workspace must not be selected when deleting it. Terraform also prevents deletion of a workspace that still has managed resources in its state.

## Using Workspace Names in Configuration

Terraform provides `terraform.workspace` to reference the currently selected workspace.

This can be used to select environment-specific values or create resource names that include the workspace name.

Example:

```hcl
variable "ami_id" {
  type = string
}

variable "instance_type" {
  type = map(string)

  default = {
    dev  = "t3.micro"
    test = "t3.small"
    prod = "t3.medium"
  }
}

resource "aws_instance" "app" {
  ami           = var.ami_id
  instance_type = var.instance_type[terraform.workspace]

  tags = {
    Name        = "app-${terraform.workspace}"
    Environment = terraform.workspace
  }
}
```

In this example:

- The selected workspace determines the instance type.
- The resource name includes the workspace name.
- The `ami_id` variable must be supplied for the configuration.
- The workspace name must match a key in the `instance_type` map.

For example, selecting the `dev` workspace uses the `t3.micro` instance type.

## Workspaces and State

Each workspace has a separate state, so Terraform in one workspace does not manage the resources tracked in another workspace's state.

For local state, Terraform stores additional workspace state files under:

```text
terraform.tfstate.d/
```

A simplified local directory structure may look like this:

```text
terraform-project/
|
|-- main.tf
|-- variables.tf
|-- outputs.tf
|-- terraform.tfstate
|
|-- terraform.tfstate.d/
    |
    |-- dev/
    |   |-- terraform.tfstate
    |
    |-- test/
    |   |-- terraform.tfstate
    |
    |-- prod/
        |-- terraform.tfstate
```

The `default` workspace uses the primary state file. Additional workspaces have separate state files.

For remote backends, Terraform manages workspace-specific state locations according to the backend's behavior and configuration.

Use a remote backend with secure access and state locking when collaborating with a team.

## Workspace Workflow Example

A typical workflow for using workspaces might look like this:

### Step 1: Initialize Terraform

```bash
terraform init
```

### Step 2: Create a Development Workspace

```bash
terraform workspace new dev
```

### Step 3: Confirm the Selected Workspace

```bash
terraform workspace show
```

### Step 4: Review the Proposed Changes

```bash
terraform plan
```

### Step 5: Apply the Configuration

```bash
terraform apply
```

### Step 6: Switch to Another Workspace

```bash
terraform workspace new test
```

Review the plan and apply the configuration for that workspace as needed.

**Important:** Always verify the selected workspace before running `terraform plan` or `terraform apply`, especially when working with production infrastructure.

## When Are Workspaces Useful?

Terraform CLI workspaces can be useful for:

- Creating temporary environments for testing.
- Running multiple instances of a similar configuration.
- Testing infrastructure changes in an isolated state.
- Managing simple environment variations where the configuration and access model are shared.

## When Should We Avoid Workspaces?

Workspaces are not a complete environment-isolation strategy.

Avoid relying on CLI workspaces when environments require:

- Different credentials or access controls.
- Strong separation between production and non-production.
- Independent architectural lifecycles.
- Separate Terraform configurations and backends.

For these cases, separate configuration directories and backends may provide clearer boundaries. Reusable modules can reduce duplication across those configurations.

## Common Mistakes

### 1. Assuming Workspaces Automatically Configure Environments

Creating a workspace does not automatically change infrastructure settings.

Use variables or variable files to provide environment-specific configuration.

### 2. Applying Changes in the Wrong Workspace

Running `terraform apply` in the wrong workspace can affect the wrong set of resources.

Check the active workspace before planning and applying:

```bash
terraform workspace show
terraform plan
```

### 3. Treating Workspaces as a Security Boundary

Workspaces separate state, but they do not inherently provide separate credentials or access controls.

Use an appropriate environment and access design for production workloads.

### 4. Using One State for Unrelated Infrastructure

Workspaces are intended for multiple instances of the same configuration. They do not replace clear state boundaries between unrelated systems.

### 5. Deleting a Workspace Without Checking Its State

Before deleting a workspace, verify that it is not managing resources that are still needed.

Terraform will not delete a workspace that still has managed resources in its state.

## Best Practices

- Use clear and consistent workspace names.
- Verify the selected workspace before planning or applying.
- Use variables or variable files for environment-specific values.
- Use remote state and locking for team-based workflows.
- Do not treat workspaces as a replacement for security boundaries.
- Keep state boundaries aligned with infrastructure ownership and lifecycle.
- Use separate configurations and backends when environments need stronger isolation.

## Quick Reference

| Command | Purpose |
|---|---|
| `terraform workspace list` | List available workspaces |
| `terraform workspace new <name>` | Create a workspace and switch to it |
| `terraform workspace select <name>` | Switch to an existing workspace |
| `terraform workspace show` | Display the current workspace |
| `terraform workspace delete <name>` | Delete an eligible workspace |

## Key Takeaway

Terraform CLI workspaces provide separate state for multiple instances of the same configuration.

They are convenient for simple variations and temporary environments, but they do not replace architectural separation, independent credentials, or access controls.

**Architect-level insight:** Choose workspaces based on the infrastructure's lifecycle and isolation needs. For production environments with different access requirements or operational ownership, separate configurations and backends may provide a clearer boundary.

<img width="1223" height="1286" alt="Terraform workspaces" src="https://github.com/user-attachments/assets/7c7f86d2-53da-43f3-9eb7-2f4d49ce57bc" />
