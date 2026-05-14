# System Design

A curated collection of system design concepts, distributed systems patterns, scalability strategies, architecture notes, and real-world design examples.

This repository is intended for:
- Software Engineers
- Solution Architects
- Backend Developers
- Cloud Engineers
- Interview Preparation
- High Scale System Learning

---

# Table of Contents

- [Introduction](#introduction)
- [System Design Fundamentals](#system-design-fundamentals)
- [Architecture Patterns](#architecture-patterns)
- [Scalability](#scalability)
- [Distributed Systems](#distributed-systems)
- [Database Design](#database-design)
- [Caching](#caching)
- [Messaging & Event Driven Architecture](#messaging--event-driven-architecture)
- [API Design](#api-design)
- [Security](#security)
- [Networking](#networking)
- [Cloud & Infrastructure](#cloud--infrastructure)
- [Containerization & Orchestration](#containerization--orchestration)
- [Observability](#observability)
- [Performance Engineering](#performance-engineering)
- [System Design Case Studies](#system-design-case-studies)
- [Interview Preparation](#interview-preparation)
- [Useful Resources](#useful-resources)

---

# Introduction

System design is the process of designing scalable, reliable, maintainable, and high-performance software systems.

This repository focuses on:
- Real-world architecture patterns
- Distributed system fundamentals
- Performance optimization
- Cloud-native engineering
- Scalability techniques
- Production-grade system design concepts

---

# System Design Fundamentals

## Topics

- CAP Theorem
- ACID vs BASE
- Horizontal vs Vertical Scaling
- Latency vs Throughput
- Availability & Reliability
- Fault Tolerance
- Stateless vs Stateful Services
- Monolith vs Microservices
- High Availability (HA)
- Disaster Recovery (DR)

---

# Architecture Patterns

## Covered Patterns

- Layered Architecture
- Clean Architecture
- Hexagonal Architecture
- CQRS
- Event Sourcing
- Saga Pattern
- Sidecar Pattern
- API Gateway Pattern
- Backend for Frontend (BFF)
- Strangler Fig Pattern

---

# Scalability

## Topics

- Load Balancing
- Auto Scaling
- Rate Limiting
- Throttling
- CDN Strategies
- Partitioning
- Database Sharding
- Read Replicas
- Multi-Region Deployment
- Geo Replication

---

# Distributed Systems

## Concepts

- Consensus Algorithms
- Distributed Locks
- Leader Election
- Idempotency
- Retry Patterns
- Circuit Breaker
- Bulkhead Pattern
- Service Discovery
- Distributed Transactions
- Eventual Consistency

---

# Database Design

## SQL Databases

- PostgreSQL
- SQL Server
- MySQL

## NoSQL Databases

- Cosmos DB
- MongoDB
- Cassandra
- Redis

## Topics

- Indexing
- Partition Keys
- UUID Strategies
- Replication
- Data Modeling
- Query Optimization
- Transactions
- Time-Series Data
- Multi-Tenant Design

---

# Caching

## Topics

- Redis
- Distributed Caching
- Cache Aside Pattern
- Write Through
- Write Back
- Cache Invalidation
- CDN Caching
- Session Caching

---

# Messaging & Event Driven Architecture

## Tools

- Kafka
- RabbitMQ
- Azure Service Bus

## Topics

- Event Driven Systems
- Pub/Sub
- Queue Based Communication
- Dead Letter Queue
- Retry Handling
- Outbox Pattern
- Event Replay

---

# API Design

## REST

- REST Principles
- Versioning
- Pagination
- Filtering
- HATEOAS

## GraphQL

- Schema Design
- Resolvers
- Federation

## gRPC

- Unary Communication
- Streaming
- Protocol Buffers

---

# Security

## Topics

- OAuth2
- OpenID Connect
- JWT
- API Security
- mTLS
- Encryption
- Secrets Management
- Zero Trust Architecture
- Rate Limiting
- WAF

---

# Networking

## Topics

- DNS
- CDN
- NAT
- VPN
- TCP/IP
- HTTP/HTTPS
- TLS Handshake
- Reverse Proxy
- Load Balancers
- Network Routing

---

# Cloud & Infrastructure

## Platforms

- Azure
- AWS
- GCP

## Topics

- Infrastructure as Code
- Terraform
- ARM/Bicep
- High Availability
- Multi Region Architecture
- Cost Optimization
- Serverless
- Cloud Security

---

# Containerization & Orchestration

## Docker

- Multi-stage Builds
- Docker Networking
- Docker Volumes
- Image Optimization

## Kubernetes

- Pods
- Deployments
- Services
- Ingress
- ConfigMaps
- Secrets
- HPA
- Service Mesh
- Helm Charts

---

# Observability

## Topics

- Logging
- Monitoring
- Distributed Tracing
- Metrics
- Alerting
- OpenTelemetry
- Grafana
- Prometheus
- ELK Stack

---

# Performance Engineering

## Topics

- Profiling
- Memory Optimization
- CPU Optimization
- Benchmarking
- Load Testing
- Concurrency
- Async Processing
- Connection Pooling

---

# System Design Case Studies

## Examples

- URL Shortener
- Chat Application
- Payment Gateway
- Ride Sharing System
- Notification Service
- Video Streaming Platform
- E-commerce Platform
- Food Delivery System
- Distributed File Storage
- Real-Time Analytics Platform

---

# Preparation

## Topics

- HLD (High Level Design)
- LLD (Low Level Design)
- Scalability Questions
- Tradeoff Discussions
- Estimation Techniques
- Real-world Scenarios

---

# Important Rules

**Rule #1**
If we are dealing with a read-heavy system, it’s good to consider using a Cache

**Rule #2**
If we need low latency in system, it’s good to consider using a Cache & CDN.

**Rule #3**
If we are dealing with a write-heavy system, it’s good to consider using a Message Queue for Async processing

**Rule #4**
If we need a system to be ACID complaint, we should go for RDBMS or SQL Database.

**Rule #5**
If data is unstructured & doesn’t require ACID properties, we should go for NO- SQL Database.

**Rule #6**
If the system has complex data in the form of videos, images, files etc, we should go for Blob/Object storage.

**Rule #7** If the system requires complex pre-computation like a news feed, we should consider using a Message Queue & Cache

**Rule #8** If the system requires searching data in high volume, we should consider using a search index, tries or search engine like Elasticsearch.

**Rule #9** If the system requires to Scale SQL Database, we should consider using Database Sharding.

**Rule #10** If the system requires High Availability, Performance, and Throughput, we should consider using a Load Balancer.

**Rule #11** If the system requires faster data delivery globally, reliability, high availability, and performance, we should consider using a CDN.

**Rule #12** If the system has data with nodes, edges, and relationships like friend lists, and road connections, we should consider using a Graph Database

**Rule #13** If the system needs scaling of various components like servers, databases, etc, we should consider using Horizontal Scaling.

**Rule #14** If the system requires high performing database queries, we should consider using Database Indexes.

**Rule #15** If the system requires bulk job processing, we should consider using Batch Processing & Message Queues.

**Rule #16** If the system requires reducing server load and preventing DOS attacks, we should consider using a Rate Limiter.

**Rule #17** If the system has microservices, we should consider using an API Gateway (Authentication, SSL Termination, Routing etc).

**Rule #18** If the system has a single point of failure, we should implement Redundancy in that component.

**Rule #19** If the system needs to be fault-tolerant, and durable, we should implement Data Replication (creating multiple copies of data on different servers)

**Rule #20** If the system needs user-to-user communication (bi-directional) in a fast way, we should consider using Websockets.

**Rule #21** If the system needs the ability to detect failures in a distributed system, we should consider implementing Heartbeat.

**Rule #22** If the system needs to ensure data integrity, we should consider implementing Checksum Algorithm.

**Rule #23** If the system needs to transfer data between various servers in a decentralized way, we should go for Gossip Protocol.

**Rule #24** If the system needs to scale servers with add/removal of nodes efficiently, no hotspots, we should implement Consistent Hashing.

**Rule #25** If the system needs anything to deal with a location like maps, nearby resources, we should consider using Quadtree, Geohash etc.
