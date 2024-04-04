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

Understanding how API consumers use their domain and identify their unmet needs is crucial for API producers. This insight allows API producers to enhance their domain. It also helps discover new domain owners to connect their domain into the supergraph.

This necessitates 2 key capabilities of the supergraph platform to create a consumer-first, agile culture:
1. API consumption, API schema & portal analytics: A supergraph is analogous to a marketplace and needs to provide the marketplace owners and producers with insights to help improve the marketplace for the consumers.
3. Ecosystem integrations: The supergraph platform should be able to integrate with existing communication and catalog tools, in particular to help understand _unmet_ demand of API consumers.

**This closes the loop and allows the supergraph platform to create a vicious cycle of success for producers and consumers**.

## Supergraph reference architecure

### CI/CD and build system (control plane)
The control plane of the supergraph is critical to help domain owners [connect their domains](#i.-connect-domains) to the supergraph.

There are 3 components in the control plane of the supergraph
1. The domain itself
2. The subgraph
3. The supergraph

<img width="800" alt="Supergraph control plane components" src="https://github.com/hasura/supergraph-io/assets/131160/e661efd8-9d0f-4340-a5a1-a119e9fc87ee">

The control plane should define the following SDLC to help keep the supergraph in sync with the domain as the underlying domain changes.

<img width="800" alt="Supergraph CI/CD" src="https://github.com/hasura/supergraph-io/assets/131160/e6fec5a3-e3da-447e-9ac3-5dc0ceef66d9">

### Distributed data plane
The supergraph data plane is critical to enable high performance access to upstream domains so that API producers can maintain their domain without hidden future maintenance costs:

<img  width="800" alt="Supergraph data plane" src="https://github.com/hasura/supergraph-io/assets/131160/c6e1de9b-fe8f-4f9e-8503-7f655b02d9a9">

### Supergraph API schema

#### Standardization

A supergraph API schema should create standardized conventions on the following:
- Queryable models vs Commands
  - Models are collections of data that can be queried in standardized ways
  - Commands are methods that map to particular pieces of business logic that might return references to other commands or models
- Standardized conventions on queryable model: While each model might only expose some ways of querying it, the syntax and conventions for standard query operations should be standardized
  - Joins
  - Filtering
  - Pagination
  - Sorting
  - Aggregations

#### Composability

The supergraph API is typically a GraphQL / JSON API. There are varying degrees of composability an API can offer, as listed out in the following table:

| Composability Attribute | Capability | Description |
| :-- | :-- | :-- | 
| C1 | Joining data | Join related data together in a "foreign key" like join |
| C2 | Nested filtering | Filter a parent by a property of its child (aka a property of a related entity) |
| C3 | Nested sorting | Sort a parent by a property of its child (aka a property of a related entity) |
| C4 | Nested pagination | Fetch a paginated list of parents, along with a paginated &amp; sorted list of children for each parent |
| C5 | Nested aggregation | Aggregate a child in the context of its parent |

These composability attributes are what increase the level of self-serve composition and reduce the need for manual API aggregation and composition.
