# How Terraform Actually Works (init → plan → apply)

## Introduction

After writing Terraform code, the next question is:

**How does Terraform actually create infrastructure?**
 
The answer lies in three core commands that make up every Terraform workflow:

- `terraform init`
- `terraform plan`
- `terraform apply`

Understanding what happens during each stage is essential for working with Terraform in production environments.

---

# The Terraform Workflow

```text
Write Terraform Code
        │
        ▼
terraform init
        │
        ▼
terraform plan
        │
        ▼
Review Changes
        │
        ▼
terraform apply
        │
        ▼
Infrastructure Created
```

Every infrastructure deployment follows this predictable workflow.

---

# Step 1: terraform init

Before Terraform can create infrastructure, it needs to prepare the working directory.

```bash
terraform init
```

## What Happens?

Terraform:

- Downloads required providers
- Installs provider plugins
- Configures the backend
- Initializes the working directory

Think of it as preparing all the necessary tools before starting construction.

---

## Why is init Required?

Terraform doesn't include every cloud provider by default.

When you run `terraform init`, it downloads only the providers your project requires.

For example:

- AWS Provider
- Azure Provider
- Google Cloud Provider

It also prepares backend configuration if remote state is being used.

---

# Step 2: terraform plan

Once the project is initialized, Terraform calculates what changes are needed.

```bash
terraform plan
```

This is one of Terraform's most powerful commands.

---

## What Happens During Plan?

Terraform compares:

```text
Terraform Configuration
            VS
Current Infrastructure State
```

It then generates an execution plan showing exactly what will happen.

Possible actions include:

- Create new resources
- Modify existing resources
- Destroy resources that are no longer required

Nothing is changed at this stage.

Terraform simply shows the proposed changes.

---

## Example Output

```text
Plan:

+ Create EC2 Instance
~ Update Security Group
- Delete Old S3 Bucket
```

This allows engineers to verify infrastructure changes before they are executed.

---

# Step 3: terraform apply

After reviewing the plan, the changes can be applied.

```bash
terraform apply
```

Terraform executes the approved execution plan.

---

## What Happens During Apply?

Terraform:

- Calls cloud provider APIs
- Creates resources
- Updates existing resources
- Deletes obsolete resources
- Records infrastructure details in the state file

After completion, the actual infrastructure matches the Terraform configuration.

---

# Behind the Scenes

The complete process looks like this:

```text
Terraform Code
        │
        ▼
terraform init
        │
Downloads Providers
Initializes Backend
        │
        ▼
terraform plan
        │
Reads State File
Compares Desired vs Current State
Generates Execution Plan
        │
        ▼
terraform apply
        │
Calls Cloud APIs
Creates/Updates Infrastructure
Updates State File
```

---

# Why Review Before Apply?

One of Terraform's biggest advantages is that changes are reviewed before execution.

Without planning:

```text
Code
   │
   ▼
Infrastructure
```

With Terraform:

```text
Code
   │
   ▼
Execution Plan
   │
Review
   │
Approval
   │
Apply
```

This significantly reduces deployment mistakes.

---

# Why This Workflow Matters

Imagine manually creating:

- VPC
- Subnets
- Internet Gateway
- Route Tables
- Security Groups
- EC2 Instances
- Load Balancers

Now imagine repeating those same steps for:

- Development
- Testing
- UAT
- Production

Terraform replaces repetitive manual work with a repeatable workflow.

---

# Best Practices

Always follow this sequence:

1. Write Terraform code
2. Run `terraform init`
3. Run `terraform plan`
4. Review the execution plan
5. Run `terraform apply`

Avoid applying infrastructure changes without reviewing the plan first.

---

# Common Mistakes

❌ Skipping `terraform init`

This may cause provider or backend errors.

---

❌ Applying without reviewing the plan

Unexpected infrastructure changes may occur.

---

❌ Ignoring provider version changes

Always initialize again after modifying provider configurations.

---

# Key Takeaways

- `terraform init` prepares the project.
- `terraform plan` previews infrastructure changes.
- `terraform apply` executes the approved plan.
- Review infrastructure before applying changes.
- Every Terraform deployment follows the same predictable workflow.

---

# Summary

Terraform follows a simple but powerful workflow:

**Initialize → Plan → Apply**

This workflow brings predictability, visibility, and safety to infrastructure deployments.

Instead of manually creating cloud resources, engineers define infrastructure in code, review the planned changes, and allow Terraform to create or update resources consistently.

Understanding these three commands is the foundation for mastering Terraform.

---
<img width="1024" height="1536" alt="How Terraform Actually Works" src="https://github.com/user-attachments/assets/1f98da03-2ddf-4f9d-8e46-fecab85ca607" />
