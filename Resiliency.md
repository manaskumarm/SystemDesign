# Resiliency Patterns in Distributed Systems

> A practical guide to Smart Retries, Idempotency, Circuit Breakers, Dead Letter Queues (DLQ), Timeouts, and other critical resiliency patterns used in scalable systems.

---

# 📖 Introduction

Modern distributed systems are inherently unreliable.

Failures happen because of:
- network issues
- service crashes
- slow databases
- temporary outages
- traffic spikes
- dependency failures

Examples:
- payment gateway timeout
- Redis failover
- Kafka broker restart
- third-party API failure

A resilient system must:
# 👉 handle failures gracefully

instead of assuming:
```text
everything always works
```

This article explains the most important resiliency patterns used in production systems.

---

# 🧠 Why Resiliency Patterns Matter

Without resiliency mechanisms:
- retries can create outages
- slow services can block entire systems
- duplicate processing may occur
- queues may grow infinitely
- failures cascade across services

Resiliency patterns help systems become:
- stable
- fault tolerant
- scalable
- recoverable

---

# 🚀 1. Smart Retries

# What is Retry?

Retry means:
# 👉 attempting an operation again after temporary failure

---

# Example

Suppose payment API fails:

```text
503 Service Unavailable
```

Instead of failing immediately:

```text
Retry request
```

---

# When to Use Retries

Retries are useful for:
- temporary network issues
- timeout failures
- transient cloud failures
- overloaded services
- temporary DB locks

---

# Good Retry Candidates

✅ HTTP 503  
✅ HTTP 429  
✅ network timeout  
✅ temporary Redis failure  
✅ Kafka reconnect  

---

# Bad Retry Candidates

❌ validation errors  
❌ authentication failures  
❌ malformed requests  
❌ business rule violations  

---

# 🚀 Exponential Backoff

Instead of retrying immediately:

```text
1s → 2s → 4s → 8s
```

This prevents retry storms.

---

# 🚀 Jitter

Jitter adds randomness:

```text
3.1s
4.7s
5.2s
```

instead of all clients retrying simultaneously.

---

# Real-World Example

Food ordering system:

```text
Payment gateway temporarily slow
```

Use:
- retry
- exponential backoff
- jitter

to avoid overloading payment provider.

---

# 🧠 Important Rule

Retries should handle:
# 👉 transient failures only

---

# 🚀 2. Idempotency

# What is Idempotency?

An operation is idempotent if:
# 👉 repeating it multiple times produces the same result

---

# Example

Suppose client retries:

```http
POST /orders
```

because network timeout occurred.

Without idempotency:
```text
duplicate orders created
```

Very dangerous.

---

# Solution

Use:
```text
Idempotency-Key
```

Example:

```http
Idempotency-Key: abc123
```

Server stores processed request result.

If same request arrives again:
```text
return existing response
```

instead of creating duplicate data.

---

# Real-World Scenarios

Critical for:
- payment processing
- order creation
- loyalty transactions
- inventory reservation

---

# Example

User clicks:
```text
"Place Order"
```

5 times because UI freezes.

Without idempotency:
```text
5 orders created
```

With idempotency:
```text
1 order created
```

---

# 🚀 3. Circuit Breaker

# What is Circuit Breaker?

Circuit breaker prevents:
# 👉 repeatedly calling failing services

---

# Why Needed?

Suppose:

```text
Payment Service DOWN
```

Without circuit breaker:

```text
Millions of requests continue hitting failed service
```

Result:
- wasted resources
- cascading failures
- thread exhaustion

---

# Circuit Breaker States

| State | Meaning |
|---|---|
| Closed | Normal operation |
| Open | Failures detected, stop calls |
| Half-Open | Test recovery with limited requests |

---

# Flow

```text
Failures increase
→ circuit opens
→ requests fail fast
→ recovery test
→ circuit closes
```

---

# Real-World Example

Food app:
```text
Loyalty service unavailable
```

Instead of blocking checkout:
- open circuit
- continue checkout without loyalty

System remains operational.

---

# Benefits

✅ prevents cascading failures  
✅ reduces overload  
✅ faster failure handling  
✅ improves resilience  

---

# 🚀 4. Dead Letter Queue (DLQ)

# What is DLQ?

DLQ stores:
# 👉 messages that repeatedly fail processing

---

# Example

Worker processing:

```text
OrderCreated event
```

fails 5 times.

Instead of retrying forever:
```text
move message to DLQ
```

---

# Why DLQ is Important

Without DLQ:
- poison messages block queues
- infinite retries occur
- systems become unstable

---

# Real-World Example

Suppose malformed menu data:

```json
{
  "price": "INVALID"
}
```

Worker crashes every retry.

Move to DLQ for manual investigation.

---

# Common Systems Supporting DLQ

- Kafka
- RabbitMQ
- SQS
- Azure Service Bus

---

# When to Use DLQ

✅ async processing  
✅ event-driven systems  
✅ background workers  
✅ retryable workflows  

---

# 🚀 5. Timeouts

# What is Timeout?

Timeout limits:
# 👉 how long a system waits for response

---

# Why Important?

Without timeouts:

```text
requests wait forever
```

Causes:
- thread exhaustion
- memory pressure
- cascading latency

---

# Example

API calls payment service:

```text
wait max 3 seconds
```

If exceeded:
```text
fail request
```

---

# Real-World Example

Checkout API:
- payment timeout = 5 sec
- loyalty timeout = 2 sec

Avoids entire checkout hanging indefinitely.

---

# Important Principle

# Fast failure is often better than slow failure.

---

# 🚀 6. Bulkhead Pattern

# What is Bulkhead?

Bulkhead isolates failures between components.

Inspired by:
> ship compartments preventing flooding spread.

---

# Example

Suppose:
```text
Notification service overloaded
```

Without isolation:
```text
entire API thread pool exhausted
```

With bulkhead:
```text
notification failures isolated
```

---

# Real-World Example

Separate thread pools for:
- payments
- notifications
- analytics

Prevents one dependency affecting others.

---

# 🚀 7. Rate Limiting

# What is Rate Limiting?

Limits:
# 👉 number of requests allowed

---

# Example

```text
100 requests/sec per client
```

---

# Why Needed?

Protects against:
- abuse
- DDoS
- overload
- retry storms

---

# Real-World Example

Food ordering app during IPL match:
```text
traffic spike
```

Rate limiting protects APIs.

---

# 🚀 8. Back Pressure

# What is Back Pressure?

Prevents:
# 👉 fast producers overwhelming slow consumers

---

# Example

Queue receives:
```text
100k msgs/sec
```

Workers process:
```text
10k msgs/sec
```

Back pressure:
- throttles producers
- protects system stability

---

# Real-World Example

Menu sync jobs flooding worker queue.

---

# 🚀 9. Health Checks

# What are Health Checks?

Endpoints used to determine:
# 👉 service health

Examples:
```http
/health
/ready
/live
```

---

# Types

| Type | Purpose |
|---|---|
| Liveness | Is process alive? |
| Readiness | Can handle traffic? |

---

# Real-World Example

Kubernetes removes unhealthy pods automatically.

---

# 🚀 10. Graceful Degradation

# What is Graceful Degradation?

System continues operating with reduced functionality.

---

# Example

Loyalty service fails.

Still allow:
```text
food ordering
```

while temporarily disabling:
```text
reward points
```

---

# Why Important?

Critical systems should not completely fail because:
```text
non-critical dependency unavailable
```

---

# 🚀 11. Queue-Based Load Leveling

# What is It?

Queues absorb traffic spikes.

---

# Example

Sudden IPL traffic:

```text
1M order requests
```

Queue smooths processing rate.

Workers process gradually.

---

# Benefits

✅ absorbs spikes  
✅ protects downstream systems  
✅ improves stability  

---

# 🚀 12. Autoscaling

# What is Autoscaling?

Automatically adjusts infrastructure capacity.

---

# Example

During dinner peak:
```text
scale API pods from 10 → 50
```

---

# Common Metrics

- CPU
- memory
- queue depth
- request rate
- Kafka lag

---

# 🚀 13. Cooldown

# What is Cooldown?

Waiting period after scaling action.

Prevents:
```text
rapid scale up/down oscillation
```

---

# Example

Scale out:
```text
wait 5 mins before next scaling decision
```

---

# 🚀 Real-World Food Ordering Architecture

```text
Client
   ↓
API Gateway
   ↓
Order Service
   ↓
Kafka/RabbitMQ
   ↓
Background Workers
   ↓
DLQ for failed events
```

Resiliency mechanisms:
- retries
- backoff
- jitter
- idempotency
- circuit breaker
- timeouts
- DLQ

all work together.

---

# 🧠 How These Patterns Work Together

Example flow:

```text
Request fails
→ retry with backoff+jitter
→ timeout if too slow
→ circuit breaker if repeated failures
→ idempotency prevents duplicates
→ DLQ handles poison messages
```

---

# 📊 Pattern Summary Table

| Pattern | Purpose | Common Use Case |
|---|---|---|
| Retry | Recover transient failures | Network timeout |
| Backoff | Prevent retry storms | API retries |
| Jitter | Spread retries randomly | Large distributed systems |
| Idempotency | Prevent duplicate processing | Payments/orders |
| Circuit Breaker | Stop hitting failed service | Dependency outages |
| DLQ | Store failed messages | Async workers |
| Timeout | Prevent hanging requests | External API calls |
| Bulkhead | Isolate failures | Thread pool isolation |
| Rate Limiting | Protect overload | Public APIs |
| Back Pressure | Control producer speed | Queues/streams |
| Graceful Degradation | Partial functionality | Optional services |
| Autoscaling | Dynamic scaling | Traffic spikes |

---

# 🎯 Key Takeaways

## Distributed systems WILL fail.

Design assuming:
- networks fail
- services crash
- retries happen
- latency spikes occur

---

# Use retries carefully

Always combine:
- retries
- exponential backoff
- jitter

---

# Idempotency is critical

Especially for:
- payments
- orders
- financial operations

---

# Circuit breakers prevent cascading failures

Fail fast instead of exhausting resources.

---

# DLQs are essential for async systems

Never retry poison messages forever.

---

# Timeouts protect system resources

Never allow requests to wait indefinitely.

---

# Resiliency is a system-wide strategy

These patterns work best:
# 👉 together

not individually.

---

# 🧠 Final Summary

Scalable systems are not just:
# 👉 fast systems

They are:
# 👉 resilient systems

The best architectures are designed to:
- tolerate failures
- recover automatically
- isolate problems
- degrade gracefully
- protect downstream systems

because in distributed systems:
# 👉 failure is not exceptional

it is normal.
