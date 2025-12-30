<img width="1749" height="961" alt="image" src="https://github.com/user-attachments/assets/701d7ae8-d85b-492e-95e6-bcc71be9e7aa" />

# Horizontal Pod Autoscaler (HPA)

## What is HPA?

Horizontal Pod Autoscaler (HPA) is a **Kubernetes native autoscaling mechanism** that automatically **increases or decreases the number of pod replicas** based on observed metrics.

👉 HPA **scales pods horizontally** (replica count), **not CPU/Memory size**.

---

## Why HPA is needed (Production Context)

In production, application traffic is never constant:

* User traffic spikes
* API requests increase suddenly
* CPU / Memory usage changes dynamically

Without HPA:

* Pods may crash under load ❌
* Resources may stay idle and waste money ❌

With HPA:

* Pods scale automatically ✅
* High availability is maintained ✅
* Cost is optimized ✅

---

## HPA Architecture (Simple View)

### Core Components

1. **Application Pods** – Actual workload (Deployment / StatefulSet)
2. **Metrics Source** – Where metrics come from
3. **HPA Controller** – Kubernetes control plane component
4. **Kube Controller Manager** – Applies scaling decisions

---

## How HPA Works (Step-by-Step Flow)

1. Application is running with a fixed number of pods (e.g., 2 pods)
2. Metrics are collected periodically (CPU, Memory, or custom metrics)
3. HPA controller compares **current metrics vs target value**
4. If metric > threshold → scale up pods
5. If metric < threshold → scale down pods
6. Kubernetes updates the replica count in Deployment

⏱️ HPA checks metrics roughly every **15 seconds**

---

## Formula Used by HPA (Important for Interviews)

Desired Replicas = Current Replicas × (Current Metric Value / Target Metric Value)

Example:

* Current pods: 2
* Current CPU usage: 80%
* Target CPU: 50%

Desired Replicas = 2 × (80 / 50) = ~4 pods

---

## Metrics Used by HPA

HPA can scale based on **three types of metrics**:

1. Resource Metrics
2. Custom Metrics
3. External Metrics

---

## 1️⃣ Resource Metrics (Default)

### What are Resource Metrics?

Built-in metrics provided by Kubernetes:

* CPU utilization
* Memory utilization

### Source

* **Metrics Server**

### Example Use Case

* Scale pods when CPU > 70%
* Most common production setup

### Limitations

* CPU/Memory may not represent real load
* Not suitable for queue-based systems

---

## 2️⃣ Custom Metrics

### What are Custom Metrics?

Metrics generated **inside Kubernetes objects**, not default CPU/Memory.

Examples:

* Requests per second
* Active HTTP connections
* Application latency

### Source

* Prometheus
* Prometheus Adapter

### How it Works

1. Application exposes metrics (/metrics)
2. Prometheus scrapes metrics
3. Prometheus Adapter exposes metrics to Kubernetes API
4. HPA reads these metrics and scales pods

### Example Use Case

* Scale pods based on API request rate
* Better scaling accuracy than CPU

---

## 3️⃣ External Metrics

### What are External Metrics?

Metrics that come from **outside Kubernetes cluster**.

Examples:

* AWS SQS queue length
* Kafka consumer lag
* RabbitMQ messages
* Cloud monitoring systems

### Source

* KEDA
* Cloud provider metrics
* External metric adapters

### Key Feature

✅ Can scale pods **from zero to N**

### Example Use Case

* Event-driven workloads
* Background workers
* Message consumers

---

## Custom vs External Metrics (Very Important)

| Feature        | Custom Metrics | External Metrics |
| -------------- | -------------- | ---------------- |
| Source         | Inside cluster | Outside cluster  |
| Example        | HTTP requests  | Queue length     |
| Prometheus     | Yes            | Optional         |
| Scale to zero  | No             | Yes              |
| Used with KEDA | Sometimes      | Mostly           |

---

## HPA in Real Production (Best Practice)

* HPA is always enabled
* CPU-based HPA for simple services
* Custom metrics for APIs
* External metrics for event-driven workloads
* Node autoscaling handled separately (Karpenter / Cluster Autoscaler)

---

## Interview One-Liners

* HPA scales pods horizontally based on metrics
* It does not create nodes
* CPU-based HPA is most common
* Custom metrics improve accuracy
* External metrics are used for event-driven scaling

---

