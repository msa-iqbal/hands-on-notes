# Architecture

Practical, structured notes for understanding, designing, scaling, communicating, and evolving **software systems and distributed architectures**.

## Contents

|#|Topic|What You'll Learn|
|---|---|---|
|01|System Design|System design fundamentals, requirements, capacity estimation, components, data flow, architecture, and the system design process|
|02|Software Architecture|Layered, client-server, three-tier, hexagonal, clean, onion, and event-driven architectures and architecture principles|
|03|API Design|API fundamentals, resource design, versioning, pagination, filtering, error handling, idempotency, rate limiting, and API best practices|
|04|Scalability|Horizontal and vertical scaling, load balancing, auto scaling, database scaling, read/write scaling, and scalability patterns|
|05|Reliability|Availability, fault tolerance, redundancy, failover, health checks, retries, timeouts, circuit breakers, and resilience patterns|
|06|Caching|Cache fundamentals, caching strategies, invalidation, TTL, distributed caching, and caching patterns|
|07|Queues|Queue fundamentals, producers and consumers, ordering, delivery, retries, dead-letter queues, backpressure, priority queues, and queue patterns|
|08|Messaging|Messaging fundamentals, pub/sub, message brokers, event streaming, synchronous and asynchronous messaging, delivery semantics, and messaging patterns|
|09|Microservices|Service boundaries, communication, service discovery, API gateways, data management, distributed transactions, Saga, observability, and microservices patterns|
|10|Distributed Systems|Distributed computing, consistency, availability, partition tolerance, CAP theorem, consensus, leader election, distributed locking, replication, partitioning, and eventual consistency|
|11|Domain-Driven Design|Domain modeling, entities, value objects, aggregates, repositories, domain services, domain events, bounded contexts, context mapping, and DDD patterns|

## Structure

The notes progress from **system design fundamentals → software architecture → API design → scalability → reliability → caching → queues → messaging → microservices → distributed systems → domain-driven design**.

The first section establishes the foundations of system design, including requirements analysis, functional and non-functional requirements, capacity estimation, system components, data flow, and the overall system design process.

Software architecture then focuses on the major architectural styles and principles used to structure applications and systems. This includes layered architecture, client-server architecture, three-tier architecture, hexagonal architecture, clean architecture, onion architecture, and event-driven architecture.

API design covers how application interfaces are structured and operated, including resource modeling, versioning, pagination, filtering, error handling, idempotency, rate limiting, and practical API design principles.

Scalability focuses on designing systems that can handle increasing workloads through horizontal and vertical scaling, load balancing, auto scaling, database scaling, and read/write scaling strategies.

Reliability covers the techniques used to keep systems available and resilient when components fail. Topics include fault tolerance, redundancy, failover, health checks, retries, timeouts, circuit breakers, and resilience patterns.

Caching explains how frequently accessed data can be stored closer to consumers to improve performance and reduce system load. The notes cover common caching strategies, cache invalidation, TTL, distributed caching, and practical caching patterns.

Queues introduce asynchronous workload processing through producers, consumers, message ordering, delivery mechanisms, retries, dead-letter queues, backpressure, and priority queues.

Messaging expands these concepts into broader communication architectures, including publish/subscribe, message brokers, event streaming, synchronous and asynchronous communication, message delivery semantics, and event-driven systems.

Microservices focuses on decomposing applications into independently deployable services. The notes cover service boundaries, service communication, service discovery, API gateways, data management, distributed transactions, Saga patterns, circuit breakers, observability, and common microservices patterns.

Distributed systems then explores the fundamental challenges of coordinating independent components across a network. Topics include consistency, availability, partition tolerance, CAP theorem, consensus, leader election, distributed locking, replication, partitioning, eventual consistency, and distributed-systems patterns.

The final section introduces domain-driven design as a way to model complex software around business domains. It covers domain models, entities, value objects, aggregates, repositories, domain services, domain events, bounded contexts, context mapping, and DDD patterns.

Each topic is separated into focused notes for easier learning, reference, practical application, system-design preparation, and future expansion.

## Focus

- System design fundamentals
- System requirements
- Functional requirements
- Non-functional requirements
- Capacity estimation
- System components
- Data flow
- System architecture
- System design process
- Software architecture
- Layered architecture
- Client-server architecture
- Three-tier architecture
- Hexagonal architecture
- Clean architecture
- Onion architecture
- Event-driven architecture
- Architecture principles
- API design
- Resource design
- API versioning
- Pagination
- Filtering and sorting
- API error handling
- Idempotency
- Rate limiting
- API best practices
- Scalability fundamentals
- Horizontal scaling
- Vertical scaling
- Load balancing
- Auto scaling
- Database scaling
- Read scaling
- Write scaling
- Scalability patterns
- Reliability fundamentals
- Availability
- Fault tolerance
- Redundancy
- Failover
- Health checks
- Retries
- Timeouts
- Circuit breakers
- Resilience patterns
- Caching fundamentals
- Cache-aside
- Read-through caching
- Write-through caching
- Write-behind caching
- Cache invalidation
- TTL
- Distributed caching
- Caching patterns
- Queue fundamentals
- Producers and consumers
- Message ordering
- Message delivery
- Retries
- Dead-letter queues
- Backpressure
- Priority queues
- Queue patterns
- Messaging fundamentals
- Publish/subscribe
- Message brokers
- Event streaming
- Synchronous messaging
- Asynchronous messaging
- Message delivery semantics
- Event-driven systems
- Messaging patterns
- Microservices fundamentals
- Service boundaries
- Service communication
- Service discovery
- API gateways
- Microservices data management
- Distributed transactions
- Saga pattern
- Microservices circuit breakers
- Microservices observability
- Microservices patterns
- Distributed systems fundamentals
- Distributed computing
- Consistency
- Availability
- Partition tolerance
- CAP theorem
- Consensus
- Leader election
- Distributed locking
- Replication
- Partitioning
- Eventual consistency
- Distributed systems patterns
- Domain-driven design
- Domain modeling
- Entities
- Value objects
- Aggregates
- Repositories
- Domain services
- Domain events
- Bounded contexts
- Context mapping
- DDD patterns

## Goal

> Build a practical architecture knowledge base that makes it easy to understand system design, structure software architectures, design robust APIs, scale applications, build reliable and resilient systems, use caching and asynchronous communication, design microservices and distributed systems, and model complex business domains using domain-driven design.
