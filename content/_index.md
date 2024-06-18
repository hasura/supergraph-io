---
title: The Supergraph Manifesto
type: docs
---

# The Supergraph Manifesto

Supergraph is an architecture framework that offers reference architectures, design guidelines/principles and an operating model to help multiple teams to collaborate on a self-serve platform for federated data access, API integration/composition or GraphQL APIs. An implementation artifact of the Supergraph architecture is called a supergraph (*lowercase `s`*).

![Before / After Supergraph](https://github.com/hasura/supergraph-io/assets/131160/2421b94e-724f-4e94-afee-61b2c81f38b7)

When a supergraph is built with a GraphQL federation stack, the engine is often called a gateway or a router and the subgraph connectors are often GraphQL services.

A supergraph is typically used for the following 2 use-cases:
1. **[Self-serve API composition platform](/docs/use-cases/api-composition)**: A self-serve operating model for API integration, orchestration & aggregation
2. **[Federated data access layer](/docs/use-cases/federated-data-access-layer)**: A federated data layer that allows realtime access to data sources with cross-domain composability (joins, filtering etc.) Related: Data mesh, data products

## Strategy and Core concepts
A supergraph approach aims to build a flywheel of data access and supply to incrementally improve self-service access to data and APIs.

<img width="660" alt="Supergraph platform flywheel" src="https://github.com/hasura/supergraph-io/assets/131160/c6583319-55d8-4854-b593-1f6c1e6b3f05">


### I. CONNECT domains
Domain owners (or data owners, or API producers) should be able to seamlessly connect their domains to the platform. A major challenge in building supergraph is the resistance to change by the domain owners. They often oppose having to build, operate and maintain another API layer, such as a GraphQL server that creates another wrapper on their domain. This reluctance and concern is understandable and completely valid and must be systematically addressed by the supergraph platform strategy and the supergraph reference architecture.

This has two main implications for the subgraph connector's lifecycle and runtime:
1. **Subgraph connector CI/CD**: As domain owners change their domains, the API contract published via the supergraph engine, must stay in sync with the least amount of overhead for the domain owner. The SDLC, change-management or CI/CD process of the domain owners must involve updating their API contract (eg: versioning), prevent breaking changes and keeping documentation up to date.
2. **Subgraph connector performance**: The subgraph connector must not _reduce_ performance as compared to what is provided by accessing the underlying domain directly. API performance characteristics as measured by latency, payload size &amp; concurrency.

Guaranteeing a smooth CI/CD process and high-performance connectivity gives domain owners confidence that they can connect their domains to the supergraph platform and iterate on changes to their domains fearlessly.

**This unlocks self-serve connectivity for domain owners.**

### II. CONSUME APIs

API consumers should be able to discover and consume APIs in a way that doesn't require manual API integration, aggregation or composition effort as far as possible. 
API consumers have several common needs when they're dealing with fixed API endpoints or specific data queries:
1. fetch different projections of data to prevent over-fetching
2. join data from multiple places to prevent under-fetching
3. filter, paginate, sort and aggregate data from multiple places

To provide an API experience that makes the consumption experience truly self-serve, there are two key requirements:
1. **Composable API design**: The API presented by the supergraph engine must allow for on-demand composability. GraphQL is a great API to express composability semantics, but regardless of the API format used, a standardized, composable API design is a critical requirement.
2. **API portal**: High-quality search, discovery and documentation of both the API and the underlying API models is critical to enable self-serve consumption. The more information that can be made available to API consumers the better. Eg: Data lineage, Authorization policies etc as appropriate.

**This unlocks self-serve consumption for API consumers**

### III. DISCOVER demand

Understanding how API consumers use their domain and identify their unmet needs is crucial for API producers. This insight allows API producers to enhance their domain. It also helps discover new domain owners to connect their domain into the supergraph.

This necessitates 2 key capabilities of the supergraph platform to create a consumer-first, agile culture:
1. API consumption, API schema &amp; portal analytics: A supergraph is analogous to a marketplace and needs to provide the marketplace owners and producers with insights to help improve the marketplace for the consumers.
2. Ecosystem integrations: The supergraph platform should be able to integrate with existing communication and catalog tools, in particular to help understand _unmet_ demand of API consumers.

**This closes the loop and allows the supergraph platform to create a virtuous cycle of success for producers and consumers**.

## Architecture guide
<!-- this section perhaps doesn't need a reference architecture beyond what's already here. It works as a guide as is.-->
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

## API schema design guide 
<!-- this section needs a reference architecture -->

### Standardization

A supergraph API schema should create standardized conventions on the following:

<table>
  <thead>
    <tr>
      <td class="fixed-col-width-1"><b>Standardization Attribute</b></td>
      <td><b>Capability</b></td>
    </tr>
  </thead>
<tbody>
  <tr>
  <td><b>S1</b></td>
  <td>
  Separating models (resources) & commands (methods)
    <details> 
      <summary>Example</summary>

  <ul>
    <li>Models are collections of data that can be queried in standardized source-agnostic ways (eg: resources)</li>
    <li>Commands are methods that map to particular pieces of business logic that might return references to other commands or models(eg: methods)</li>
  </ul>
        
```graphql
  # A standardized way to fetch a list of authors
  query GetAuthors {
    authors {
      id
      name
    }
  }

  # A specific method to search for authors
  query findAuthors {
    search_authors(args: {search: "Einstein"}) {
      id
      name
    }
  }
```
 </details>
  </td>
  </tr>

  <tr>
  <td><b>S2</b></td>
  <td>
    Model filtering
    <br/>
    <details>
      <summary>Example</summary>
      Get a list of articles published this year
      
```graphql
 query articlesThisYear {
    articles(where: {publishDate: {_gt: "2024-01-01"}}) {
      id
      name
    }
  }
```
  </details>
  </td>
  </tr>
  <tr>
  <td><b>S3</b></td>
  <td> 
    Model sorting
  <details>
    <summary>Example</summary>
  Get a list of articles sorted in reverse by the date of publishing

  ```graphql
  query sortedArticles {
    article(order_by: {publishDate: desc}) {
      id
      title
      author_id
    }
  }
  ```
  </details>
  </td>
  </tr>
  <tr>
  <td><b>S4</b></td>
  <td> 
    Model pagination
    <details>
      <summary>Example</summary>
  Paginate the above list with 20 objects per page and fetch the 3rd page

  ```graphql
  query sortedArticlesThirdPage {
    article(order_by: {publishDate: desc}, offset: 40, limit: 20) {
      id
      title
      author_id
    }
  }
  ```
  </details>
  </td>
  </tr>

  <tr>
  <td><b>S5</b></td>
  <td> 
    Model aggregations over fields
    <details>
      <summary>Example</summary>
  Get a count of authors and their average age

  ```graphql
  query authorStatistics {
    author_aggregate {
      aggregate {
        count # basic aggregation support by any model
        avg { # supported over any numeric fields of a type
          age
        }
        
      }
    }
  }
  ```
  </details>
  </td>
  </tr>
</tbody>
</table>

**Prior art**
  - [Google Cloud API design guide](https://cloud.google.com/apis/design/resources)
    - Resource: A resource-oriented API is generally modeled as a resource hierarchy, where each node is either a simple resource or a collection resource
    - Method: Resources are manipulated via a small set of methods

### Composability

The supergraph API is typically a GraphQL / JSON API. There are varying degrees of composability an API can offer, as listed out in the following table:

<table>
<thead>
<tr>
<td class="fixed-col-width-1"><b>Composability Attribute</b></td>
<td class="fixed-col-width-2"><b>Capability</b></td> <td><b>Description</b></td>
</tr>
</thead>
<tr>
<tbody>
<td><b>C1</b></td>
<td> Joining data</td>
<td>Join related data together in a "foreign key" like join
  <details>
    <summary>Example</summary>
Get a list of authors and <b>their</b> articles

```graphql
query authorWithArticles {
  author {
    id
    name
    articles {
      id
      title
    }
  }
}
```
</details>
</td>
</tr>

<tr>
<td><b>C2</b></td>
<td> Nested filtering</td>
<td>Filter a parent by a property of its child (i.e. a property of a related entity)
    <details>
    <summary>Example</summary>
Get a list of authors whose have published an article this year

```graphql
query recentlyActiveAuthors {
  author(where: {articles: {publishDate: {_gt: "2024-01-01"}}}) {
    id
    name
  }
}
```
</details>
</td>
</tr>
<tr>
<td><b>C3</b></td>
<td> Nested sorting </td>
<td>Sort a parent by a property of its child (i.e. a property of a related entity)
    <details>
    <summary>Example</summary>
Get a list of articles sorted by the names of their author

```graphql
query sortedArticles {
  article(order_by: {author: {name: asc}}) {
    id
    title
  }
}
```
</details>
</td>
</tr>
<tr>
<td><b>C4</b></td><td> Nested pagination </td>
<td>Fetch a paginated list of parents, along with a paginated &amp; sorted list of children for each parent
    <details>
    <summary>Example</summary>
Get the 2nd page of a list of authors and the first page of <b>their</b> articles, sorted by the article's title field

```graphql
query paginatedAuthorsWithSortedPaginatedArticles {
  author(offset: 10, limit: 20) {
    id
    name
    articles(offset: 0, limit: 25, order_by: {title: asc}) {
      title
      publishDate
    }
  }
}
```
</details>
</td>
</tr>
<tr>
<td><b>C5</b></td><td> Nested aggregation </td>
<td>Aggregate a child/parent in the context of its parent/child
    <details>
    <summary>Example</summary>
Get a list of authors and the number of articles written by each author

```graphql
query prolificAuthors {
  author (limit: 10) {
    id
    name
    articles_aggregate {
      count
    }
  }
}
```
</details>
</td>
</tr>
</tbody>
</table>

These composability attributes are what increase the level of self-serve composition and reduce the need for manual API aggregation and composition.
