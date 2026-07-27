# Terraform State Explained

## Introduction

One of the most important concepts in Terraform is **State**.

After running `terraform apply`, Terraform needs a way to remember the infrastructure it has created.

That's exactly what the **Terraform State file** does.

Without the state file, Terraform would have no memory of the resources it manages.

---

# What is Terraform State?

Terraform State is a file that stores information about every resource Terraform manages.

By default, Terraform stores this information in:

```text
terraform.tfstate
```

Think of it as Terraform's inventory or memory.

The state file contains:

- Resource IDs
- Resource attributes
- Dependencies
- Metadata
- Current infrastructure information

Terraform uses this information during every deployment.

---

# Why Does Terraform Need State?

Terraform is declarative.

It only knows the **desired infrastructure** described in your code.

To determine what needs to change, Terraform must compare:

```text
Terraform Configuration
        │
        ▼
Terraform State
        │
        ▼
Actual Cloud Infrastructure
```

The state file acts as the bridge between your code and the real cloud resources.

---

# How Terraform Uses the State File

Whenever you run:

```bash
terraform plan
```

Terraform performs the following steps:

1. Reads your Terraform configuration.
2. Reads the Terraform State file.
3. Compares the state with the actual cloud infrastructure.
4. Calculates the required changes.
5. Generates an execution plan.

Only the required changes are applied.

---

# Example

Suppose you create an EC2 instance.

Terraform stores information similar to:

```text
Resource: aws_instance.web

ID: i-0ab12345
Region: ap-south-1
Instance Type: t2.micro
```

During the next deployment, Terraform already knows that this EC2 instance exists.

Instead of creating another instance, it updates only the required configuration.

---

# Why Doesn't Terraform Query AWS Every Time?

A common question is:

**Why doesn't Terraform simply ask AWS what resources exist?**

Because AWS only knows what exists.

It doesn't know:

- Which resources belong to Terraform
- Which resources belong to another application
- Which resources Terraform should manage
- Which resources should be ignored

Terraform State provides that ownership information.

---

# What Happens If the State File Is Lost?

If Terraform loses its state file, it loses track of all managed resources.

This can result in:

- Duplicate resources
- Incorrect execution plans
- Recreated infrastructure
- Lost dependency information
- Deployment failures

Protecting the state file is essential.

---

# Local State vs Remote State

## Local State

```text
terraform.tfstate
```

Stored on the engineer's local machine.

Suitable only for learning or personal projects.

---

## Remote State

Stored in a shared backend such as:

- Amazon S3
- Azure Storage Account
- Google Cloud Storage
- Terraform Cloud

Recommended for all team environments.

---

# Production Best Practices

Never store production state files only on your laptop.

Instead:

- Store state remotely.
- Enable versioning.
- Secure access using IAM or RBAC.
- Encrypt the state file.
- Enable state locking.
- Back up the backend regularly.

---

# Why State Locking Matters

Imagine two engineers running:

```bash
terraform apply
```

at the same time.

Both attempt to modify the same state file.

This can corrupt the state and lead to inconsistent infrastructure.

State locking prevents simultaneous updates.

We'll cover State Locking in the next article.

---

# Behind the Scenes

```text
Write Terraform Code
        │
        ▼
Read Terraform State
        │
        ▼
Compare with Cloud
        │
        ▼
Generate Plan
        │
        ▼
Apply Changes
        │
        ▼
Update State File
```

The state file is updated after every successful deployment.

---

# Key Takeaways

- Terraform State is Terraform's memory.
- It tracks every resource Terraform manages.
- Terraform compares code, state, and cloud infrastructure before making changes.
- Losing the state file can cause duplicate resources and deployment failures.
- Production environments should always use a remote backend.
- State locking protects shared infrastructure from concurrent modifications.

---

# Summary

Terraform State is one of the core building blocks of Infrastructure as Code.

It enables Terraform to understand the current infrastructure, calculate differences, and safely apply only the required changes.

Without the state file, Terraform cannot reliably manage cloud resources.

Understanding Terraform State is essential before learning remote backends, state locking, and collaborative Terraform workflows.

---

## What's Next?

In the next article, we'll explore **Terraform State Locking** and learn how Terraform prevents multiple engineers from modifying infrastructure at the same time.

<img width="1024" height="1536" alt="Terraform Statefile Explained" src="https://github.com/user-attachments/assets/7844384c-3519-4a91-ae0a-1b1c78be0978" />
