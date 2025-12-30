# Vertical Pod Autoscaler (VPA)

## What is VPA?

Vertical Pod Autoscaler (VPA) is a Kubernetes autoscaling component that **automatically adjusts CPU and memory requests/limits of pods** based on their actual usage.

👉 VPA scales **vertically** (resource size of a pod), not the number of pods.

---

## Why VPA Exists

In real clusters:

* Developers often set **wrong CPU/Memory requests**
* Too low → pods get throttled or OOMKilled ❌
* Too high → wasted resources & higher cost ❌

VPA helps by:

* Observing real usage
* Recommending or applying better resource values

---

## What VPA Scales (Very Important)

VPA adjusts:

* `requests.cpu`
* `requests.memory`
* (optionally) limits

🚫 VPA does NOT:

* Scale pod replicas
* Create or delete nodes

---

## VPA Architecture

### Main Components

1. **VPA Recommender**

   * Continuously observes pod CPU & memory usage
   * Calculates optimal resource recommendations

2. **VPA Updater**

   * Evicts pods if resource changes are required
   * Allows pods to restart with new values

3. **VPA Admission Controller**

   * Injects recommended CPU/Memory values into new pods

---

## How VPA Works (Step-by-Step)

1. Application pods are running in the cluster
2. Metrics Server collects CPU & memory usage
3. VPA Recommender analyzes historical usage
4. VPA generates recommended resource values
5. Based on VPA mode:

   * Apply automatically
   * Or only recommend
6. Pods restart (if required) with new resources

⚠️ Pod restart is a **key behavior** of VPA

---

## VPA Modes (EXTREMELY IMPORTANT)

### 1️⃣ Off Mode

* VPA only gives recommendations
* No changes applied automatically
* Safest mode

Used when:

* Teams want visibility
* During initial tuning

---

### 2️⃣ Initial Mode

* VPA applies recommendations **only at pod creation time**
* Running pods are NOT restarted

Used when:

* You want better defaults
* Avoid pod restarts

---

### 3️⃣ Auto Mode

* VPA automatically updates resources
* Pods are evicted & restarted

Used when:

* Batch jobs
* Non-critical workloads

🚫 Rarely used for live production traffic

---

## VPA and HPA Together (Critical Rule)

❌ VPA and HPA should NOT manage the same resource

Reason:

* HPA changes replicas
* VPA changes requests
* This causes scaling conflicts

### Safe Combinations

* HPA (replicas) + VPA (recommendation only)
* HPA on custom metrics + VPA on memory (carefully)

---

## VPA in Production (Reality)

✅ Used mostly in **recommendation mode**
⚠️ Rarely used in full auto mode

### Common Production Use Cases

* Finding correct CPU/Memory requests
* Batch workloads
* CronJobs
* Internal tools

### NOT recommended for:

* High-traffic APIs
* Latency-sensitive services

---

## VPA vs HPA (Clear Difference)

| Feature          | HPA                 | VPA             |
| ---------------- | ------------------- | --------------- |
| Scales           | Pod count           | Pod size        |
| Metric           | CPU, custom         | CPU, memory     |
| Pod restart      | No                  | Yes             |
| Production usage | Very high           | Limited         |
| Best for         | APIs, microservices | Resource tuning |

---

## Interview One-Liners

* VPA scales pod resources, not replicas
* It may restart pods
* Mostly used in recommendation mode
* Not suitable for high-traffic apps
* HPA is primary autoscaler in production

---

