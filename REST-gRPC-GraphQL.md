# REST vs GraphQL vs gRPC
| Decision Axis | REST | GraphQL | gRPC |
|---|---|---|---|
| **Protocol** | HTTP/1.1 or 2, JSON | HTTP/1.1 or 2, JSON | HTTP/2, Protobuf (binary) |
| **Over-fetching** | High risk — fixed response shape | Eliminated — client picks fields | Moderate — schema-defined |
| **Under-fetching / N+1** | Common — multiple round trips | Solved — single request, DataLoader | Solved — streaming, batching |
| **Mobile clients** | OK — careful endpoint design needed | Excellent — lean payloads, one call | Limited — gRPC-web needed in browser |
| **Multiple endpoints** | Proliferates — one URL per resource | Single endpoint — `/graphql` always | Services — methods in `.proto` files |
| **Real-time / streaming** | Weak — polling or SSE | Subscriptions — via WebSocket | Native — bidirectional streams |
| **Performance / latency** | Good — JSON overhead | Good — query parsing cost | Excellent — binary, multiplexed |
| **Caching** | Native — HTTP cache, CDN out of box | Complex — POST, persisted queries | Manual — must implement yourself |
| **Type safety** | Weak — OpenAPI optional | Schema — but runtime JSON | Strict — Protobuf compiled types |
| **Developer experience** | Universal — every dev knows it | Learning curve — resolvers, schema | Steepest — proto files, codegen |
| **Third-party / public API** | Best — familiar, browser-native | Growing — GitHub, Shopify do it | Poor — not browser-native |
| **Versioning** | URL-based — `/v1`, `/v2` sprawl | Schema evolution — deprecate fields | Proto evolution — field numbers |
| **Tooling / ecosystem** | Mature — Swagger, Postman, etc. | Mature — Apollo, Relay, GraphiQL | Growing — grpcurl, BloomRPC |
| **Cost / infra** | Cheapest — CDN caches cut compute | Moderate — resolver overhead | Low compute — fewer bytes, H2 mux |
| **Resilience pattern** | Idempotency keys, retry-safe GETs | Query depth limits, persisted queries | Deadlines, retry policies in proto |

---

## Quick Recommendations

| Use Case | Recommended Technology |
|---|---|
| Public APIs | REST |
| Mobile/Web aggregation | GraphQL |
| Internal microservice communication | gRPC |
| High-performance low-latency systems | gRPC |
| Flexible frontend-driven UIs | GraphQL |
| Simple CRUD systems | REST |

---

<img width="542" height="143" alt="image" src="https://github.com/user-attachments/assets/e521e85c-17c6-4c74-946b-2f3e2aca19f4" />

## Typical Enterprise Architecture

```text
Mobile/Web
    ↓
GraphQL BFF
    ↓
REST/gRPC Microservices
    ↓
Databases
```
<img width="544" height="339" alt="image" src="https://github.com/user-attachments/assets/567d22e5-d642-403f-8dd3-b65e364a154d" />

