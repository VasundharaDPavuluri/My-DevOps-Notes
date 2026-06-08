## Poll SCM vs Webhooks: Why Webhooks Changed CI/CD

One question stood out while learning Jenkins:

How does Jenkins know that new code was pushed?

At first glance, the answer seems simple.

But the evolution of that answer reflects a broader shift in how modern systems are designed.

---

### The Early Approach: Scheduled Builds

Initially, Jenkins jobs were configured to run at fixed intervals.

Examples:

* Every 5 minutes
* Every 15 minutes
* Every hour

The problem?

Most executions found nothing new to build.

This led to:

* Unnecessary resource consumption
* Delayed feedback
* Inefficient automation

The system was working, but not intelligently.

---

### The Next Improvement: Poll SCM

To improve efficiency, Jenkins introduced Poll SCM.

Instead of running a build immediately, Jenkins periodically checked the source repository.

The workflow looked like this:

```text
Jenkins
↓
Checks GitHub
↓
Detects Change
↓
Starts Build
```

This was better than scheduled builds because deployments only happened when code changed.

However, the platform was still continuously asking:

"Has anything changed yet?"

---

### The Architectural Limitation

Polling works.

But polling does not scale efficiently.

As repositories, teams, and pipelines grow:

* More API requests are generated
* More infrastructure resources are consumed
* Feedback is delayed until the next polling cycle

The system remains reactive instead of event-driven.

---

### The Shift to Webhooks

Webhooks completely changed the model.

Instead of Jenkins checking GitHub repeatedly:

GitHub notifies Jenkins immediately when an event occurs.

```text
Developer Push
↓
GitHub Event
↓
Webhook
↓
Jenkins Pipeline Triggered
```

No waiting.

No unnecessary polling.

No wasted cycles.

---

### Why This Matters Beyond Jenkins

The interesting part is that this pattern appears everywhere in modern cloud architectures.

Examples include:

* Amazon EventBridge
* Kafka
* SNS
* SQS
* GitOps workflows
* Serverless platforms

The principle is the same:

Instead of continuously asking if something happened,

React when something actually happens.

---

### One Insight That Changed My Thinking

The real innovation wasn't faster polling.

The real innovation was moving from polling-based automation to event-driven automation.

That shift improves:

* Scalability
* Efficiency
* Responsiveness
* Resource utilization

---

### Simple Way to Remember

```text
Poll SCM
=
"Did something change?"

Webhook
=
"Something changed."
```
