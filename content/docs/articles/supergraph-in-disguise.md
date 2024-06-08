---
title: When should you supergraph?
---

# When should you supergraph?

The root problem
- Consuming data from multiple places requires integration or aggregation work
- This increases the burden on the producer since it takes time to build and is fragile
- The problem is combinatorially complex when there are multiple producers and multiple consumers

<img width="660" alt="Supergraph need" src="https://github.com/hasura/supergraph-io/assets/131160/2debe261-813a-4100-83dd-ef3efb8dc8d0">

## Common symptoms
1. API consumers are frustrated by the lack of APIs that are optimized for their specific data retrieval use cases
2. API producers are not willing to take on the burden of maintaining and operationalizing API aggregation and composition problems
3. There is no operating model or ownership model for who should solve the integration problem

## Supergraph in disguise

If you're working on any of the following projects or strategic initiatives, then you're likely already
building a supergraph.

1. You're starting to evaluate the need of a **federated data access layer**
2. API consumers wish they had a **monolithic API view of their microservice APIs**
3. API consumers want GraphQL, but **API producers don't want to maintain GraphQL** services
5. You're trying to add **API integration or orchestration capabilities to your API gateway**
6. You're thinking about **API standardization** for creating flexible data APIs
7. You want to **replace direct database access with APIs**
8. You want to deliver **data products** or a **data mesh over an API**
9. You want to **create a knowledge graph** and enable data retrieval for **AI** applications

