# Supergraph

A supergraph is an architecture pattern and a federated operating model to help teams create a self-serve platform for data access, API integration/composition or GraphQL.

![Before / After Supergraph](https://github.com/hasura/supergraph-io/assets/131160/2421b94e-724f-4e94-afee-61b2c81f38b7)

When a supergraph is built with a GraphQL federation stack, the engine is ofted called a gateway or a router and the subgraph connectors are often GraphQL services.

## Supergraph platform strategy
A supergraph approach aims to build a flywheel of growth to keep improving self-service access to data and APIs.

<img width="660" alt="Supergraph platform flywheel" src="https://github.com/hasura/supergraph-io/assets/131160/c6583319-55d8-4854-b593-1f6c1e6b3f05">

### I. CONNECT domains
Domain owners (aka data owners or API producers) should be able to seamlessly connect their domains to the platform. One of the major challenges to watch out for while building supergraphs is that domain owners are highly resistant to change (and rightly so) and do not want to maintain another API layer (eg: a GraphQL server that creates another wrapper on their domain) since it dramatically increases their build & operate burden. 

This has 2 main implications:
1. **Subgraph connector CI/CD**: As domain owners change their domains, the API contract published via the supergraph engine, must stay in sync with the least amount of overhead for the domain owner. The SDLC, change-management or CI/CD process of the domain owners must involve updating their API contract (versioning), prevent breaking changes and keeping documentation up to date.
2. **Subgraph connector performance**: The subgraph connector must not _reduce_ performance as compared to what is provided by accessing the underlying domain directly. API performance characteristics as measured by latency, payload size & concurrency.

Guaranteeing a smooth CI/CD process and high-performance connectivity gives domain owners confidence that they can connect their domains to the supergraph platform and iterate on changes to their domains fearlessly.

**This unlocks self-serve connectivity for domain owners.**

### II. CONSUME APIs

API consumers should be able to discover and consume APIs in a way that doesn't require manual API integration, aggregation or composition effort as far as possible. 
API consumers need different projections of data (over-fetching), or need to join data from multiple places (under-fetching), or filter, paginate, sort and aggregate data served by discrete endpoints or data access queries.

To provide an API experience that makes the consumption experience truly self-serve, there are a 2 key requirements:
1. **Composable API design**: The API presented by the supergraph engine must allow for on-demand composability. GraphQL is a great API to express composability semantics, but regardless of the API format used, a standardized, composable API design is a critical requirement.
2. **API portal**: High-quality search, discovery and documentation of both the API and the underlying API models is critical to enable self-serve consumption. The more information that can be made available to API consumers the better. Eg: Data lineage, Authorization policies etc as appropriate.

**This unlocks self-serve consumption for API consumers**

### III. DISCOVER demand

Helping producers understand how consumers are using their domain and what they need but aren't able to get is critical to help producers improve their domain and to help the organization understand which producers need to connect their domains to the supergraph. 

This requires 2 key capabilities to create a consumer-first, agile culture:
1. API consumption, API schema & portal analytics: A supergraph is analogous to a marketplace and needs to provide the marketplace owners and producers with insights to help improve the marketplace for the consumers.
3. Ecosystem integrations: The supergraph platform should be able to integrate with existing communication and catalog tools, in particular to help understand _unmet_ demand of API consumers.

**This closes the loop and allows the supergraph platform to create a vicious cycle of success for producers and consumers**.

## Supergraph reference architecure

### Control plane
The control plane of the supergraph is critical to help domain owners [connect their domains](#i.-connect-domains) to the supergraph.



### Distributed data plane

### Supergraph API schema

### Ecosystem integration

## Reference implementation
