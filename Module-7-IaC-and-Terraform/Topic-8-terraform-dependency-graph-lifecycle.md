# Terraform Dependency Graph & Lifecycle

## Overview

When we write Terraform code, resources are not necessarily created in the order they appear in the `.tf` file.

For example, an EC2 instance may depend on:

```text
VPC
 ↓
Subnet
 ↓
Security Group
 ↓
EC2 Instance
```

Terraform needs to understand these relationships before creating the infrastructure.

This is where the **Terraform Dependency Graph** becomes important.

---

## 1. What Is a Dependency?

A dependency exists when one resource needs another resource to exist first.

For example:

```text
VPC
 ↓
Subnet
 ↓
EC2 Instance
```

Terraform understands that the EC2 instance must wait for the required infrastructure.

---

## 2. Implicit Dependencies

Terraform automatically identifies many dependencies through resource references.

For example:

```hcl
resource "aws_subnet" "app" {
  vpc_id = aws_vpc.main.id
}
```

The subnet references the VPC.

So Terraform understands:

```text
aws_vpc.main
      ↓
aws_subnet.app
```

This is called an **implicit dependency**.

We don't need to manually define the creation order.

---

## 3. Explicit Dependencies

Sometimes Terraform cannot understand a dependency through a direct resource reference.

In those cases, we can explicitly define it using `depends_on`:

```hcl
resource "example_resource" "app" {
  depends_on = [
    aws_instance.database
  ]
}
```

This tells Terraform that the resource depends on the database.

However, `depends_on` should not be used everywhere.

If Terraform can understand the relationship through resource references, that is usually the cleaner approach.

---

## 4. The Terraform Dependency Graph

Terraform builds a graph of resources and their relationships.

For example:

```text
        VPC
       /   \
   Subnet   Security Group
       \   /
      EC2 Instance
```

Terraform uses this graph to understand:

- What depends on what
- What can be created in parallel
- What needs to wait
- What needs to be updated
- What can be safely removed

Terraform is therefore not simply executing the configuration from top to bottom.

---

## 5. What Is Terraform Lifecycle?

Dependencies define the relationship between resources.

**Lifecycle rules** control how Terraform handles changes to resources.

For example:

```hcl
lifecycle {
  create_before_destroy = true
}
```

This tells Terraform to create the replacement resource before destroying the existing one.

Conceptually:

```text
Create New Resource
        ↓
Replacement Ready
        ↓
Destroy Old Resource
```

Instead of:

```text
Destroy Old Resource
        ↓
Create New Resource
```

This can help reduce downtime during resource replacement.

---

## 6. Why Lifecycle Rules Matter

Infrastructure changes do not always mean creating something completely new.

Sometimes Terraform needs to replace an existing resource.

Lifecycle rules allow us to control how these changes should happen.

They can be useful when:

- Replacing infrastructure
- Reducing downtime
- Preventing accidental destruction
- Controlling resource replacement behaviour

The important point is that lifecycle rules influence **how Terraform changes infrastructure**.

---

## Dependency Graph vs Lifecycle

These two concepts solve different problems.

```text
Dependency Graph
       ↓
What depends on what?
       ↓
Resource relationships
```

```text
Lifecycle
       ↓
How should changes happen?
       ↓
Resource change behaviour
```

Understanding this difference makes Terraform configurations easier to reason about.

---

## Architect-Level Perspective

Terraform is not simply reading configuration and executing commands in sequence.

It builds an understanding of:

```text
Resources
    +
Dependencies
    +
Desired State
```

It then calculates how to move the infrastructure from its current state to the desired state.

This is important when managing larger environments where resources have many relationships and changes need to be controlled carefully.

---

## My Key Takeaway

The **Dependency Graph** answers:

> What depends on what?

Lifecycle rules answer:

> How should Terraform handle changes?

Together, they help Terraform manage infrastructure changes in a controlled way.

---

## Simple Way to Remember

```text
Dependency Graph
        ↓
Resource Relationships

Lifecycle
        ↓
Change Behaviour

Terraform
        ↓
Current State → Desired State
```

Terraform uses these concepts to understand infrastructure relationships and determine how changes should be applied.

---

## Key Takeaways

- Terraform builds a dependency graph for resources.
- Resource references create implicit dependencies.
- `depends_on` can be used for explicit dependencies when required.
- Terraform can create independent resources in parallel.
- Lifecycle rules control how resource changes are handled.
- `create_before_destroy` can help reduce downtime during replacement.
- Dependency Graph and Lifecycle solve different problems.
- Understanding both helps us design more predictable Terraform configurations.
---
<img width="912" height="899" alt="Terraform-infra creation" src="https://github.com/user-attachments/assets/99ffe899-e96f-4d25-9f14-18f4207daafa" />
