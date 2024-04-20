# The Supergraph Manifesto

Supergraph is an architecture framework that offers reference architectures, design guidelines/principles and an operating model to help mulitple teams to collaborate on a self-serve platform for federated data access, API integration/composition or GraphQL APIs. An implementation artifact of the Supergraph architecture is called a supergraph (*lowercase `s`*).

![Before / After Supergraph](https://github.com/hasura/supergraph-io/assets/131160/2421b94e-724f-4e94-afee-61b2c81f38b7)

When a supergraph is built with a GraphQL federation stack, the engine is often called a gateway or a router and the subgraph connectors are often GraphQL services.

## Strategy and Core concepts
A supergraph approach aims to build a flywheel of data access and supply to incrementally improve self-service access to data and APIs.

<img width="660" alt="Supergraph platform flywheel" src="https://github.com/hasura/supergraph-io/assets/131160/c6583319-55d8-4854-b593-1f6c1e6b3f05">


### I. CONNECT domains
Domain owners (aka data owners or API producers) should be able to seamlessly connect their domains to the platform. A major challenge in building supergraph is the resistance to change by the domain owners. They often oppose having to build, operate and maintain another API layer, such as a GraphQL server that creates another wrapper on their domain. This reluctance and concern is understandable and completely valid and must be systematically addressed by the supergraph platform strategy and the supergraph reference architecture.

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
<!-- this section needs a reference schema -->
High-quality platform APIs are possible only with high quality subgraphs. This architecture framework offers the following design guidelines for subgraphs and a supergraph quality benchmark. 

### Subgraph Standardization
A supergraph should help create standardized conventions for subgraphs APIs on the following:

#### Queryable models vs Commands
  - Models are collections of data that can be queried in standardized ways
  - Commands are methods that map to particular pieces of business logic that might return references to other commands or models

This is analogous to how tables represent a collection of "resources" and functions (whether in code or in the DB itself) represent logic performed on a resource or a set of resources (typically to mutate their state). This is also based on the core principles behind REST - model is a resource (noun) and commands (verbs) typically change the state of resources.

In a high-quality supergraph, subgraphs on heterogenous sources or domains should be standardised to represent data as models and/or commands regardless of the underlying source as shown in the [reference Supergraph implementation](hasura.io/ddn) below:


<table>
<tr>
<td>Source</td> <td>Entity</td> <td>Definition</td> <td>GraphQL query</td><td>Response </td>
</tr>
<tr>
<td> Postgres </td>
<td>
table: authors
</td>
<td>

```sql
(
  id SERIAL PRIMARY KEY, 
  name VARCHAR (50)
);
```

</td>
<td>

```graphql
query {
  author {
    id
    name
  }
}
```
</td>
<td>

```json
{
  "data": {
    "author": [
      {
        "id": 1,
        "name": "Newton"
      }
  ]
  }
}
```
</td>
</tr>
<tr>
<td> Mongo </td>
<td>
collection: movies
</td>
<td>

```
{
    _id: ObjectId
    title: String
    plot: String
    genres: Array (...)
    ...
}
```
</td>
<td>

```graphql
query {
  movies {
    _id
    title
    plot
    genres
  }
}
```
</td>
<td>

```json
{
  "data": {
    "movies": [
      {
        "_id": {
          "$oid": "5733..."
        },
        "title": "Great Train...",
        "plot": "Bandits...",
        "genres": [
          "Short",
          "Western"
        ]
      }
    ]
}
}
```
</td>
</tr>
</table>

**Prior art**
  - [Google Cloud API design guide](https://cloud.google.com/apis/design/resources)
    - Resource/Model: A resource-oriented API is generally modeled as a resource hierarchy, where each node is either a simple resource or a collection resource. 
    - Method/Command: resources are manipulated via a small set of methods
  - [Github API]
    - [collection/models](https://docs.github.com/en/rest/pulls/pulls?apiVersion=2022-11-28): `GET /repos/{owner}/{repo}/pulls` lists pull requests in a specified repository. A `pull request` is a model. 
    - [command](https://docs.github.com/en/rest/pulls/pulls?apiVersion=2022-11-28#merge-a-pull-request): `PUT /repos/{owner}/{repo}/pulls/{pull_number}/merge` merges a pull request into the base branch.

#### Standardized conventions on a queryable model
While each model might only expose some ways of querying it, the syntax and conventions for standard query operations should be standardized, along with the requirement to support as many of the following features:
  - Filtering
  - Pagination
  - Sorting
  - Aggregations
  - Joins

> [!IMPORTANT]
> Features like filtering and aggregation are critical for supporting mature composition use-cases (*see section on composability*). In real-world scenarios, they are also used together. E.g. Filtering by aggregate value (<i>List of articles which have an average rating of more than 3/5</i>)


#### Reference subgraph schema
The following is a reference implementation for a subgraph schema i.e. a GraphQL schema for a table `authors` and a database function `search_author` that returns a search (*you can extrapolate this to existing REST APIs and other entities in your domain*). This reference schema supports the aforementioned standardization on the model-command design as well as on the subgraph conventions outlined above.

<table>
<tr>
<td><b>Principle/Feature</b></td> <td><b>Reference type definition</b></td> <td><b>Example type definition</b></td> <td><b>Example query</b></td>
</tr>

<tr>
<td>
    Type definition
</td> 
<td>

```graphql
Type type_name {
field1: scalarType1!
field2: scalarType2
}
``` 

</td> 

<td>

```graphql
Type Author {
id: Int!
name: String
}
``` 

</td> 
<td>
NA
</td>
</tr>
<tr>
<td>
    <b>Collection</b> (<i>with filtering, pagination, sorting and limits<i>)
</td> 
<td>

```graphql
field(
distinct_on: [field_select_column!]
limit: Int
offset: Int
order_by: [field_order_by!]
where: field_bool_exp
): [Type!]!
``` 
</td> 

<td>

```graphql
author(
distinct_on: [author_select_column!]
limit: Int
offset: Int
order_by: [author_order_by!]
where: author_bool_exp #defined below
): [author!]!

author_bool_exp #argument type defination
_and: [author_bool_exp!]
_not: author_bool_exp
_or: [author_bool_exp!]
id: Int_comparison_exp #defined below
name: String_comparison_exp

Int_comparison_exp #argument type defination
_eq: Int
_gt: Int
_gte: Int
_in: [Int!]
_is_null: Boolean
_lt: Int
_lte: Int
_neq: Int
_nin: [Int!]
``` 

</td> 
<td>

```graphql
query filteredAuthors {
  author(where: {id: {_gt: 100}}, limit: 10) {
    id
    name
  }
}
```

</td>
</tr>

<tr>
<td>
    <b>Collection - aggregates</b>
</td> 
<td>

```graphql
field_aggregate(
distinct_on: [field_select_column!]
limit: Int
offset: Int
order_by: [field_order_by!]
where: field_bool_exp
): field_aggregateType!
``` 
</td> 

<td>

```graphql
author_aggregate(
distinct_on: [author_select_column!]
limit: Int
offset: Int
order_by: [author_order_by!]
where: author_bool_exp
): author_aggregate! #defined below

author_aggregate #argument type defination
aggregate: author_aggregate_fields #defined below
nodes: [author!]!

author_aggregate_fields #argument type defination
avg: author_avg_fields
count(columns: [author_select_column!]distinct: Boolean): Int!
max: author_max_fields
min: author_min_fields
stddev: author_stddev_fields
``` 

</td> 
<td>

```graphql
query filteredAuthorAggregate {
  author_aggregate(where: {name: {_like: " Curie"}}) {
    nodes {
      id
      name
    }
    aggregate {
      count
    }
  }
}
```
</td>
</tr>

<tr>
<td>
    <b>Model</b>
</td> 
<td>

```graphql
field_by_pk(id_field: scalarType1!): type
``` 
</td> 

<td>

```graphql
author_by_pk(id: Int!): author
``` 

</td> 
<td>

```graphql
query author {
  author_by_pk(id: 10) {
    id
    name
  }
}
```

</td>
</tr>

<tr>
<td>
    <b>Command</b>
</td> 
<td>

```graphql
command_name (
args: command_args!
distinct_on: [model_select_column!]
limit: Int
offset: Int
order_by: [field_order_by!]
where: field_bool_exp
): [Type!]!
``` 
</td> 

<td>

```graphql
search_authors(
args: search_authors_args!
distinct_on: [author_select_column!]
limit: Int
offset: Int
order_by: [author_order_by!]
where: author_bool_exp
): [author!]!
``` 

</td> 
<td>

```graphql
query findAuthors {
  search_authors(args: {search: "Einstein"}) {
    id
    name
  }
}
```

</td>
</tr>



</table>


#### Subgraph schema quality checklist


| Sno. | Quality criterion| Check (<i>Yes/ No / Partially<i>) |
| :-- | :-- | :-- |
| 1 | Does the subgraph implement a models & commands API design | |
| 2 | Do the collections in the subgraph support filtering<sup>*</sup>  | |
| 3 | Do the collections in the subgraph support pagination<sup>*</sup> | |
| 4 | Do the collections in the subgraph support sorting<sup>*</sup> | |
| 5 | Do the collections in the subgraph support aggregates<sup>*</sup> | |
| 6 | Can you filter collections by aggregate values of model attributes | |

<sup>*</sup> <i>with standard conventions</i>

### Composability

The supergraph API is typically a GraphQL / JSON API that composes over multiple underlying domains. There are varying degrees of composability a supergraph API can offer, as listed out in the following table. The table uses a typical authors, articles, and ratings schema (which can be derived from databases or existing domain APIs). `authors`<>`articles` have a 1:many relationship, and `articles`<>`ratings` have a 1:1 relationship. 


<table>
<tr>
<td><b>Composability Attribute</b></td> <td><b>Capability</b></td> <td><b>Description</b></td> <td><b>Example with query</b></td>
</tr>
<tr>
<td><b>C1</b></td><td> Joining data</td> <td>Join related data together in a "foreign key" like join</td> 
<td>
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
</td>
</tr>

<tr>
<td><b>C2</b></td><td> Nested filtering</td> <td>Filter a parent by a property of its child (<i>aka a property of a related entity</i>)</td> 
<td>
Get a list of authors whose have published an article this year

```graphql
query recentlyActiveAuthors {
  author(where: {articles: {publishDate: {_gt: "2024-01-01"}}}) {
    id
    name
  }
}
```
</td>
</tr>
<tr>
<td><b>C3</b></td><td> Nested sorting </td> <td>Sort a parent by a property of its child(<i>aka a property of a related entity</i>)</td> 
<td>
Get a list of articles sorted by the names of their author

```graphql
query sortedArticles {
  article(order_by: {author: {name: asc}}) {
    id
    title
  }
}
```
</td>
</tr>
<tr>
<td><b>C4</b></td><td> Nested pagination </td> <td>Fetch a paginated list of parents, along with a paginated &amp; sorted list of children for each parent</td> 
<td>
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
</td>
</tr>
<tr>
<td><b>C5</b></td><td> Nested aggregation </td> <td>Aggregate a child/parent in the context of its parent/child</td> 
<td>
Get a list of prolific authors who have written more than a <b>total</b> of 50 articles

```graphql
query prolificAuthors {
  author(where: {articles_aggregate: {count: {predicate: {_gt: 50}}}}) {
    id
    name
  }
}
```
</td>
</tr>
</table>

> [!IMPORTANT]  
> These composability attributes are what increase the level of self-serve composition and reduce the need for manual API aggregation and composition. The key to supporting better composability lies in the quality of the subgraph and your ability to support composition on data relationships. For example, in the previous section, we looked at a collection that supports filtering on its own attributes. To support C2 or C5, let's say, the types for that collection's `where` clause will need to go further and also support boolean expressions comprising of nested field's attributes:

<table>
<tr>
<td><b>Before</b></td><td><b>After</b></td>
</tr>
<tr>
<td>

```graphql
author_bool_exp
_and: [author_bool_exp!]
_not: author_bool_exp
_or: [author_bool_exp!]
id: Int_comparison_exp
name: String_comparison_exp
```

</td>
<td>

```graphql
author_bool_exp
_and: [author_bool_exp!]
_not: author_bool_exp
_or: [author_bool_exp!]
id: Int_comparison_exp
name: String_comparison_exp
articles: article_bool_exp # new argument type
articles_aggregate: article_aggregate_bool_exp # new argument type


article_bool_exp # new argument type definition
_and: [article_bool_exp!]
_not: article_bool_exp
_or: [article_bool_exp!]
author_id: Int_comparison_exp
id: Int_comparison_exp
publishDate: date_comparison_exp
title: String_comparison_exp
```
</td>
</table>

> [!WARNING]  
> With some GraphQL federation approaches, this degree of composition is impossible to implement without spinning up new domains for every such permutatio. This is because type definition boundaries are prematurely set when defining a subgraph. A connector based approach can lazy load the definitions allowing for greater flexibility in defining compositions with no additional overhead.

## More reading

- [Use cases](/use-cases)
- [FAQ](/faq)
