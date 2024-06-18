---
title: Federated Data Access Layer
---

# Federated Data Access Layer

A federated data access layer (FDAL) is a data access layer that allows flexible and secure access to multiple, independently governed data sources.

## Typical consumers

FDAL consumers range from consumers that need low-latency, high-concurrency access to realtime data to consumers that need access to analytical data.

- Experience layer products & microservices
- BFFs - Backend for frontends
- Internal products & services - reporting, BI etc.
- 3rd party developers
- Emerging: LLMs, AI applications

![Federated data access layer consumers](https://github.com/hasura/supergraph-io/assets/131160/a1056635-3f45-4a7a-aac1-b1f4bf5a6e39)

## Benefits of a FDAL

1. **Decrease cost, retire legacy / redundant technology and increase efficiency:**
    - Avoid moving data around across multiple platforms and layers - especially premature ETL
    - Create data consistency and quality controls
    - Prevent data duplication
2. **Improve compliance and governance:**
    - Enforce a consistent, and if required, a centralized authorization model
    - Realtime data access: Provide access to data as soon as it is created
    - Create a standardized API for different types of workloads
3. **Improve productivity and collaboration:**
    - Provide a stable API to decouple storage layer & physical modelling from domain modelling
    - Provide an operating model to incorporate cross-domain business logic, often referred to as the "process layer" or "orchestration layer"
    - Codify performance & stability expectations as SLAs
    - Incoporate marketplace dynamics: usage analytics and producer / consumer collaboration to drive improvements in the underlying data sources

## Supergraph platform checklist

There are 6 key aspects to building a federated data access layer with a supergraph reference architecture.
A supergraph platform and its technology stack must
support the following capabilities in order to help create a federated data access layer.

### I. Unified semantic model

A unified semantic model to understand the various entities in the supergraph (resources, business methods, authorization policies).

### II. Data modelling & business logic

Subgraph connectors should allow domain or API modelling by leveraging the language of the underlying data-source, while allowing the addition of business logic to handle transformation, validation & authorization.

This ensures subgraphs are "thin" in that its performance characteristics are determined by that of the underlying data source in highly predictable ways.
This also allows subgraphs to be "thick" enough to incorporate the required business logic to expose stable domain concepts.

### III. Authorization

An authorization policy engine that supports:
- Rule / policy composition
- Field (column) level visbility
- Entity (row) level access control
- Optimized integration with data fetching (eg: projection & predicate push-down)

### IV. API design

A standardized API design that can handle (domain driven):
1. Querying resources & invoking business methods
2. Filtering, sorting, pagination & aggregation operations on resources
3. Conventions for relational, event, KV and graph workloads

Aggregation & orchestration layer API needs like (consumer driven):
1. Joining data
2. Nested filtering, sorting pagination & aggregations
3. Orchestration business logic that requires querying resources or invoking methods across multiple domains

### V. Performance & observability

Query planning:
1. Visiblity / explainability in lower environments (development, staging)
2. Tracing in production
3. Data source specific optimizations to reduce load on the upstream data source

Latency tax:
1. Measure and optimize added latency on top of the underlying data source

### VI. Federated ownership

- Independent CI/CD for domain (data source) owners
- Composability (aggregation, orchestration) across domains
- Analytics & collaboration tooling for producers and consumers
