# Observability in Microservices

> A practical guide to Logs, Metrics, Distributed Tracing, TraceId propagation, and monitoring microservices in a real-world food ordering system.

---

# 📚 Table of Contents

- [What is Observability?](#-what-is-observability)
- [Why Observability is Important](#-why-observability-is-important)
- [The 3 Pillars of Observability](#-the-3-pillars-of-observability)
  - Logs
  - Metrics
  - Traces
- [What is Distributed Tracing?](#-what-is-distributed-tracing)
- [TraceId, SpanId and Parent Span](#-traceid-spanid-and-parent-span)
- [Food Ordering System Example](#-food-ordering-system-example)
- [How Trace Propagation Works](#-how-trace-propagation-works)
- [Passing TraceId Across Services](#-passing-traceid-across-services)
- [Observability Libraries & Tools](#-observability-libraries--tools)
- [Azure Application Insights](#-azure-application-insights)
- [Industry Best Practices](#-industry-best-practices)
- [Common Production Problems](#-common-production-problems)
- [Recommended Architecture](#-recommended-architecture)
- [Summary](#-summary)

---

# 🚀 What is Observability?

Observability means:

> Understanding what is happening inside a system using outputs like logs, metrics and traces.

In microservices architecture:
- one request touches multiple services
- failures happen in multiple places
- debugging becomes difficult

Observability helps engineers:
- detect failures
- identify bottlenecks
- trace user requests
- monitor system health
- debug production faster

---

# 🤔 Why Observability is Important

Consider a food ordering application:

```text
Client
   ↓
Order Service
   ↓
Payment Service
   ↓
Loyalty Service
```

Suppose users report:

```text
"Order placed but loyalty points missing"
```

Questions:
- Did payment fail?
- Was loyalty service down?
- Was database slow?
- Was network timeout happening?
- Which service caused the issue?

Without observability:

```text
debugging becomes guesswork
```

---

# 🧱 The 3 Pillars of Observability

| Pillar | Purpose |
|---|---|
| Logs | Detailed event information |
| Metrics | Numerical measurements |
| Traces | End-to-end request tracking |

---

# 📘 1. Logs

## What are Logs?

Logs are detailed records of events happening inside the system.

Example:

```json
{
  "traceId": "abc123",
  "service": "OrderService",
  "orderId": "ord-101",
  "message": "Order created successfully"
}
```

---

## Logs Help Answer

- What happened?
- Which request failed?
- What exception occurred?
- Which order or user impacted?

---

## Types of Logs

| Type | Example |
|---|---|
| Information | Order created |
| Warning | Retry happening |
| Error | Payment failed |
| Debug | Detailed troubleshooting |

---

## Logging Best Practices

### ✅ Use Structured Logging

Good:

```json
{
  "orderId": "123",
  "status": "FAILED"
}
```

Bad:

```text
Order failed for order 123
```

---

### ✅ Always Include Correlation IDs

Always log:
- traceId
- spanId
- orderId
- userId

This helps correlate logs across services.

---

### ✅ Avoid Sensitive Data

Never log:
- passwords
- tokens
- card details
- secrets

---

# 📊 2. Metrics

## What are Metrics?

Metrics are numerical measurements collected over time.

Examples:
- requests/sec
- API latency
- CPU usage
- DB query duration
- cache hit ratio

---

## Example Metrics

```text
Orders/sec = 1200
Payment latency = 350ms
Error rate = 1.2%
```

---

## Metrics Help Detect

- traffic spikes
- slow APIs
- retry storms
- infrastructure saturation
- dependency failures

---

## Golden Signals (Industry Standard)

| Signal | Meaning |
|---|---|
| Latency | Response time |
| Traffic | Request volume |
| Errors | Failure rate |
| Saturation | Resource exhaustion |

---

# 🔍 3. Traces

## What are Traces?

Tracing tracks:

> a single request across multiple services.

Example:

```text
Order Service
   ↓
Payment Service
   ↓
Loyalty Service
```

Tracing helps identify:
- where request failed
- which service is slow
- dependency bottlenecks
- end-to-end latency

---

# 🚀 What is Distributed Tracing?

Distributed tracing means:

> tracking a request across distributed microservices using a shared TraceId.

In microservices:
- one user request touches many services
- services communicate using HTTP/gRPC/messages
- failures happen in downstream dependencies

Tracing connects the complete request journey.

---

# 🧠 TraceId, SpanId and Parent Span

## TraceId

Represents:

> the complete request journey across all services.

Example:

```text
traceId = xyz-123
```

All services share the same TraceId.

---

## Span

A span represents:

> one unit of work.

Examples:
- HTTP request
- DB query
- Redis call
- external API call

---

## SpanId

Unique identifier for a span.

Example:

```text
spanId = payment-span-1
```

---

## Parent Span

Spans form parent-child relationships.

Example:

```text
Order Span
   ├── Payment Span
   └── Loyalty Span
```

This creates a complete trace tree.

---

# 🍔 Food Ordering System Example

Suppose user places an order.

---

## Step 1 — Client Calls Order API

```http
POST /orders
```

Order Service creates:

```text
TraceId = trace-123
SpanId = order-span
```

---

## Step 2 — Order Service Calls Payment Service

Order Service forwards:

```http
traceparent
```

header to Payment Service.

Payment Service creates:

```text
TraceId = trace-123
SpanId = payment-span
ParentSpan = order-span
```

---

## Step 3 — Payment Service Calls Loyalty Service

Loyalty Service creates:

```text
TraceId = trace-123
SpanId = loyalty-span
ParentSpan = payment-span
```

---

## Final Distributed Trace

```text
Client Request
TraceId = trace-123

Order Service
  Span = order-span

    ↓

Payment Service
  Span = payment-span

    ↓

Loyalty Service
  Span = loyalty-span
```

Entire user journey becomes traceable.

---

# 🌐 How Trace Propagation Works

Modern systems use:

> W3C Trace Context Standard

HTTP Header:

```http
traceparent
```

Example:

```http
traceparent: 00-abc123-payment001-01
```

This propagates:
- TraceId
- Parent Span information

across services automatically.

---

# ⚙️ Passing TraceId Across Services

In ASP.NET Core:
- HttpClient
- OpenTelemetry
- Application Insights

automatically propagate trace context.

---

## Example

```csharp
services.AddHttpClient();
```

ASP.NET Core automatically injects:

```http
traceparent
```

header into outgoing requests.

---

## Request Flow

```text
OrderService
   ↓ HttpClient
traceparent header added automatically
   ↓
PaymentService
```

Payment Service continues the same trace automatically.

---

# 🔧 Observability Libraries & Tools

## Logging Libraries

| Library | Purpose |
|---|---|
| Serilog | Structured logging |
| NLog | Logging |
| Log4Net | Logging |

---

## Distributed Tracing Libraries

| Library | Purpose |
|---|---|
| OpenTelemetry | Industry standard observability |
| System.Diagnostics.Activity | Built-in .NET tracing |
| Jaeger | Trace visualization |
| Zipkin | Distributed tracing |

---

## Monitoring & APM Tools

| Tool | Purpose |
|---|---|
| Azure Application Insights | Monitoring & APM |
| Prometheus | Metrics collection |
| Grafana | Dashboards |
| Datadog | Observability platform |

---

# ☁️ Azure Application Insights

Application Insights automatically tracks:

| Feature | Supported |
|---|---|
| Incoming HTTP requests | ✅ |
| Outgoing HTTP calls | ✅ |
| SQL queries | ✅ |
| Exceptions | ✅ |
| Distributed traces | ✅ |
| Metrics | ✅ |

---

# 🔭 Example Distributed Trace in App Insights

```text
Order API
 ├── SQL Query
 ├── Redis Call
 ├── Payment API
 │     └── Payment DB Query
 └── Loyalty API
       └── Loyalty DB Query
```

This helps quickly identify:
- slow services
- failing dependencies
- bottlenecks

---

# 🏗️ Recommended Observability Architecture

```text
Client
   ↓
API Gateway
   ↓
Order Service
   ↓
Payment Service
   ↓
Loyalty Service
```

All services should:
- propagate TraceId
- create spans
- emit logs
- publish metrics
- send telemetry to Application Insights

---

# ✅ Industry Best Practices

## Logs

- Use structured JSON logging
- Include traceId and spanId
- Avoid sensitive data
- Log business events

---

## Metrics

Monitor:
- latency
- traffic
- errors
- saturation

Configure alerts for:
- high latency
- increased failures
- retry storms

---

## Tracing

Always propagate:
- TraceId
- SpanId

Across:
- HTTP
- Kafka
- queues
- background jobs

---

## OpenTelemetry

Use OpenTelemetry because:
- vendor-neutral
- industry standard
- cloud agnostic
- portable

---

# 🚨 Common Production Problems Observability Solves

| Problem | How Observability Helps |
|---|---|
| Slow API | Trace latency breakdown |
| Payment failure | Correlated logs |
| Retry storm | Metrics spikes |
| Cache misses | Cache metrics |
| DB bottleneck | Dependency tracing |
| Timeout issues | Distributed traces |

---

# 🧠 Important Engineering Insights

## Logs Alone Are NOT Enough

Logs tell:

```text
what happened
```

but not:
- system health trends
- latency spikes

---

## Metrics Alone Are NOT Enough

Metrics show:

```text
something is wrong
```

but not:
- which request failed
- request journey

---

## Traces Connect Everything

Tracing provides:

> end-to-end visibility of user journey.

This is extremely important in microservices.

---

# 🎯 Summary

## Logs

Help understand:

```text
What happened?
```

---

## Metrics

Help understand:

```text
Is the system healthy?
```

---

## Traces

Help understand:

```text
How did the request travel across services?
```

---

# ✅ Final Takeaway

In microservices architecture:

> distributed tracing is critical.

Without:
- TraceId
- Span correlation
- centralized observability
- distributed tracing

debugging production systems becomes extremely difficult.

A strong observability strategy enables:
- faster debugging
- better reliability
- proactive monitoring
- improved customer experience
