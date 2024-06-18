# API Composition: API Integration, Aggregation & Orchestration

We use the term API composition to encompass three main aspects of working with multiple API endpoints: integration, orchestration, and aggregation.

A key driver for the Supergraph is the need for API composition. GraphQL (monolithic or federated) is a special case of this need.

## What is API composition & why is it hard?
While domain owners (producers) are owners of a domain API, in a multi-consumer and multi-producer scenario, API consumers often also need specialized APIs that are optimized for their use cases.

![Screen Shot 2024-05-13 at 10 22 07 PM](https://github.com/hasura/supergraph-io/assets/131160/53f3de0c-1f35-4032-a63d-9c2602566073)

This creates a tension between "domain-driven" API design and ownership and "consumer-driven" API design.

| Design & Ownership | Benefits | Challenges |
| ------------- | ------------- | ----- |
| **Domain-driven** | Consistent, standardized API for multiple consumers | Not optimized for consumer needs |
| **Consumer-driven** | Optimized for single consumer and can incorporate consumer-specific business logic | Hard to standardize and needs to be purpose-built for every consumer |

A supergraph allows both these API design and ownership models to co-exist. A supergraph platform brings in domain APIs via its subgraph connectors and provides a self-serve API orchestration and aggregation layer across the various domains. This allows domain owners to design and evolve their domain API, and other supergraph stakeholders to aggregate APIs on-demand and add custom orchestration workflows.

A supergraph platform solves the following three API composition problems:
1. Integration
2. Aggregation
3. Orchestration

## Solving API Integration

Given that domains and domain APIs exist, API integration remains challenging for API consumers for the following reasons:
1. The API output format or protocol is not ideal or optimal for a consumer.
2. The API does not have a typed schema and/or does not provide an SDK experience for the consumer.
3. The API's documentation is missing or out of date.
4. The API does not have standardized conventions or follow a consistent design.
5. API versioning creates tension with high-velocity development 

A supergraph provides a systematic way to address these challenges because it provides a common semantic layer and registry for the underlying domains and their APIs. A well-setup supergraph platform provides out-of-the-box solutions for the challenges mentioned above.

## Solving API Aggregation (or batching)

API consumers often need to fetch data from multiple API endpoints. API aggregation or batching, performed closer to the domains, can prevent excessive data transfer and reduce network round trips.

API aggregation is challenging because:
1. Explosive creation of new aggregation API endpoints: Different consumers have different needs, and their needs evolve rapidly in a high-velocity environment.
2. Fuzzy ownership: Domain APIs are owned and designed by domain owners, but often it is not clear who builds, designs, and operates API endpoints that aggregate data across these endpoints.

A supergraph provides a self-service model for API aggregation and batching by modeling the underlying domains as a "graph" and then allowing API consumers to fetch whatever slice of data they need on-demand without requiring the development and maintenance of new aggregate endpoints.

A well-setup supergraph platform provides a high level of composability that makes different types of API aggregation possible on demand. For example:
1. Joins: Fetch data from A and related data from B.
2. Nested filters: Fetch data from A, filtered by a property value of its related data B.

## Solving API Orchestration

API consumers often need to create reliable workflows that require sequencing multiple API calls interspersed with business logic. Even if the underlying domain APIs exist, API orchestration is challenging because it is the part of the API that is consumer-defined and potentially spans multiple domains.

This makes it challenging to create a unified technology approach and identify owners to build and operate these workflows.

Related: Sagas, Distributed transactions, state machines.

A supergraph platform should provide a clear operating model and technology best practices to manage API orchestration, beyond 
simple aggregation/batching use-cases.


## Supergraph checklist for API composition

<table>
  <!--thead>
    <tr>
      <td class="fixed-col-width-1"><b>Attribute</b></td>
      <td><b>Description</b></td>
    </tr>
  </thead-->
    <tbody>
    <tr>
        <td><b>1. Integration</b></td>
        <td><b>Making it easy for API consumers to integrate APIs into their services</b></td>
    </tr>
    <tr>
        <td>1.1 Multiple API formats</td>
        <td>
          <ul>
            <li>Can the supergraph platform to automatically provide output formats beyond GraphQL? Eg: REST/OpenAPI, gRPC.</li>
          </ul>
          This is required to prevent a lock in to the GraphQL protocol as needs change over time.
    </tr>
    <tr>
        <td>1.2 Documentation</td>
        <td>
          <ul>
            <li>Does the supergraph platform help domain owners maintain documentation? </li>
            <li>If the underlying domain (database, code or APIs) are already documented, are those automatically picked up by the supergraph platform?</li>
          </ul>
        </td>
    </tr>
    <tr>
        <td>1.3 Standardization</td>
        <td>
          <ul>
            <li>Does the supergraph platform provide or enforce a standardized domain API design? (Eg: pagination, filtering, sorting etc)</li>
          </ul>
        </td>
    </tr>
    <tr>
        <td><b>2. Aggregation</b></td>
        <td><b>Making it easy for API consumers to aggregate/batch multiple API calls into one</b></td>
    </tr>
    <tr>
        <td>2.1 Relationships</td>
        <td>
        <ul>
          <li>Does the supergraph provide a way of creating relationships between any 2 entities or endpoints without requiring changes from the domain owners</li>
        </ul>
        </td>
    </tr>
    <tr>
        <td>2.2 Composability</td>
        <td>
          <ul>
            <li>How many "join" features does the supergraph provide, given a relationship between 2 entities in the supergraph? <a href="/#composability">Examples</a></li>
          </ul>
        </td>
    </tr>
    <tr>
        <td><b>3. Orchestration</b></td>
        <td><b>Making it easy for supergraph stakeholders to author custom API orchestration</b></td>
    </tr>
    <tr>
        <td>3.1 Custom orchestration business logic </td>
        <td>
          <ul>
            <li>Does the supergraph provide a way to author orchestration flows within or across underlying domains?</li>
          </ul>
        </td>
    </tr>
  </tbody>
</table>

## Federated GraphQL Anti-patterns

Building a federated GraphQL API with a supergraph is a strategic decision. When the key desired benefit of GraphQL is to solve an API composition problem on top of existing domains, then the key expected ROI is to improve API integration, aggregation, and orchestration. In this case, the following anti-patterns should be avoided. Building a federated GraphQL API with a supergraph is a strategic decision, and the wrong choice can create thousands of person-hours of technical debt and legacy that become hard to unwind.

1. ❌ **Pure schema-driven implementation**:
    - The situation: The entire GraphQL schema is hand-written to, purportedly meet the goal of a "consumer first" API design.
    - The problem: Domains and domain APIs are already designed keeping the needs of one or more consumers in mind. Recreating these parts of API in the GraphQL schema is essentially complete duplication of effort and results in the creation of a parallel standard. A consumer driven approach is only important for the parts of the API that require API aggregation and custom API orchestration workflows.
    - Symptoms:
      - A parallel API standards group starts to exist.
      - Existing domain owners are not willing to own their subgraph GraphQL servers and find it tedious.
      - A new squad or team is spending their entire development time building and maintaining a GraphQL wrapper.
    - The solution:
      - Domain-driven subgraphs: Auto-generate subgraphs that accurately reflect the domain. Subgraph improvements are driven by domain design improvements.
      - Consumer-driven additions to the supergraph: Create a tech stack and an operating model for making high-velocity additions to the supergraph based on the needs of a specific consumer.
2. ❌ **Forcing subgraph owners to own inter-subgraph relationships**:
    - The situation: Relationships between subgraphs can only be specified by subgraph owners.
    - The problem: While this approach allows subgraph owners to extend and connect their subgraphs to other subgraphs, it requires domain owners (subgraph owners) to understand other subgraphs.
    - Symptoms:
      - Disconnected supergraphs. Inefficient query execution plans if relationships are hard to implement in some directions. 
      - API consumers who are frustrated at not being able to participate in creating inter-subgraph relationships.
    - The solution: In addition to allowing subgraph owners to create inter-subgraph relationships, allow the creation of relationships in the supergraph engine outside the subgraphs as well.
3. ❌ **Requiring an additional API registry to create custom resolvers (eg: federated mutations)**:
    - The situation: Mutations can only originate from a domain subgraph.
    - The problem: Supergraph stakeholders are forced to create a new subgraph that directly connects to underlying domains. This requires stakeholders to refer to another API registry to understand how to connect to the underlying subgraph domains (eg: database, REST).
    - Symptoms: 
      - API consumers are frustrated at not being able to easily create custom resolvers that represent some unique business logic and workflow for their specific need. 
      - Lack of an operating model around federating mutations, or API sagas, or distributed transactions.
    - The solution: The supergraph platform should provide a way for supergraph stakeholders to author custom API workflows interspersed with business logic without having to refer to APIs outside of the supergraph.

