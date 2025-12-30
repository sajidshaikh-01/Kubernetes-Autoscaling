# KEDA (Kubernetes Event-Driven Autoscaling)

## What is KEDA?

KEDA (Kubernetes Event-Driven Autoscaling) is an autoscaling component that **scales Kubernetes pods based on events**, not just CPU or memory.

👉 KEDA is mainly used for **event-driven and queue-based workloads**.

Examples of events:

* Messages in AWS SQS
* Kafka consumer lag
* RabbitMQ queue length
* Azure Service Bus
* Prometheus metrics
* Cron schedules

---

## Why KEDA is Needed

HPA works well for:

* APIs
* Web applications

But HPA fails when:

* CPU usage is low but queue length is high
* Pods should scale **from 0 to N**
* Workload is event-driven

KEDA solves this problem.

---

## What KEDA Does (Very Important)

KEDA:

* Creates and manages **HPA automatically**
* Provides **external metrics** to HPA
* Allows **scale to zero**

🚫 KEDA does NOT replace HPA
👉 KEDA **uses HPA internally**

---

## KEDA Architecture

### Core Components

1. **KEDA Operator**

   * Runs inside the cluster
   * Watches ScaledObjects
   * Creates or updates HPA

2. **Metrics Adapter**

   * Fetches metrics from external systems
   * Exposes them to Kubernetes Metrics API

3. **Event Source**

   * SQS, Kafka, RabbitMQ, Prometheus, etc.

---

## How KEDA Works (Step-by-Step)

1. You deploy an application (consumer / worker)
2. You create a **ScaledObject** for that app
3. KEDA starts monitoring the event source
4. Event count increases (example: queue depth)
5. KEDA exposes this as an external metric
6. KEDA creates an HPA automatically
7. HPA scales pods based on the metric
8. When events go to zero → pods scale to zero

---

## Key KEDA Resource: ScaledObject

A **ScaledObject** defines:

* Target workload
* Event source (trigger)
* Scaling rules

Example (Conceptual):

* Scale deployment when SQS messages > 5

---

## Types of KEDA Triggers

### 1️⃣ Queue-Based Triggers

* AWS SQS
* RabbitMQ
* Kafka
* Azure Service Bus

Used for:

* Background workers
* Message consumers

---

### 2️⃣ Metrics-Based Triggers

* Prometheus metrics
* Datadog
* CloudWatch

Used for:

* Advanced autoscaling logic

---

### 3️⃣ Time-Based Triggers

* Cron

Used for:

* Scheduled jobs
* Business-hour scaling

---

## Scale-to-Zero (Major Advantage)

KEDA can:

* Scale pods from 0 → N
* Save cost when idle

HPA alone cannot do this efficiently.

---

## KEDA vs HPA (Clear Difference)

| Feature       | HPA          | KEDA   |
| ------------- | ------------ | ------ |
| Scaling basis | CPU / Memory | Events |
| Scale to zero | ❌            | ✅      |
| Built-in K8s  | ✅            | ❌      |
| Uses HPA      | N/A          | ✅      |
| Complexity    | Low          | Medium |

---

## KEDA in Production (Reality)

✅ Used for:

* Kafka consumers
* SQS workers
* Event-driven pipelines

🚫 Not used for:

* Normal APIs
* Web frontends

In production:

* HPA is default
* KEDA is **used only where needed**

---

## Interview One-Liners

* KEDA enables event-driven autoscaling
* It works by creating HPA automatically
* Supports scale-to-zero
* Used for queue and stream processing

---

