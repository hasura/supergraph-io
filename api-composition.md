# API Composition: API Integration, Aggregation & Orchestration

We use the term API composition to encompass three main aspects of working with multiple API endpoints:
integration, orchestration and aggregation.

A key driver for the Supergraph is the need for API composition. GraphQL (monolithic or federated) is a special case of this need.

While domain owners (producers) are owners of a domain API, in a multi-consumer & multi-producer scenario, API consumers often also need specialized APIs that are optimized for their use-cases.

This creates a tension between "domain driven" API design and ownership and "consumer driven" API design.

| Design & Ownership | Benefits | Challenges |
| ------------- | ------------- | ----- |
| **Domain driven** | Consistent, standardized API for multiple consumers | Not optimized for consumer needs |
| **Consumer driven** | Optimized for single consumer and can incorporate consumer specific business logic | Hard to standardize and needs to be purpose-built for every consumer |
 

A supergraph allows both these API design and ownership models to co-exist. 
A supergraph platform brings in domain APIs via its subgraph connectors and provides a self-serve API orchestration & aggregation layer across the various domains. This allows domain owners to design and evolve their domain API, and other supergraph stakeholders to aggregate APIs on-demand and add custom orchestration workflows.

A supergraph platform solves the following 3 API composition problems:
1. Integration
2. Aggregation
3. Orchestration

## Integration

Given that domains and domain APIs exist, API integration remains challenging for API consumers for the following reasons:
1. The API output format or protocol is not ideal or optimal for a consumer
2. The API does not have a typed schema and/or does not provide a SDK experience for the consumer
3. The API's documentation is missing or out of date
4. The API does not have standardized conventions or follow a consistent design

A supergraph provides a systematic way to address these challenges because it provides a common semantic layer and registry for the underlying domains and their APIs.
A well setup supergraph platform provides out of the box solutions for the challenges mentioned above.

## Aggregation (or batching)

API consumers often need to fetch data from multiple API endpoints.
API aggregation or batching, performed closer to the domains can prevent excessive data transfer and prevent network round trips.

API aggregation is challenging because:
1. Explosive creation of new aggregation API endpoints: Different consumers have different needs and their needs evolve rapidly in a high-velocity environment
2. Fuzzy ownership: Domain APIs are owned and designed by domain owners but often it is not clear who builds, designs and operates API endpoints that aggregate data across these endpoints

A supergraph provides a self-service model for API aggregation and batching by modelling the underlying domains as a "graph" and then allowing API consumers to fetch whatever
slice of data they need from the on-demand without requiring the development and maintenance of new aggregate endpoints.

A well setup supergraph platform provides a high level of composability that makes different types of API aggregation possible on demand.
Eg:
1. Joins: Fetch data from A and related data B
2. Nested filters: Fetch data from A, filtered by a property value of its related data B.

## Orchestration

API consumers often need to create reliable workflows that require sequencing multiple API calls interspersed with business logic.
Even if the underlying domain APIs exist, API orchestration is challenging because it is the part of the API that is consumer defined
and potentially spans multiple domains. 

This makes it challenging to create a unified technology approach and identify owners to build and operate these workflows.

Related: Sagas, Distributed transactions, state machines.

A supergraph platform should provide a clear operating model and technology best practices to manage API orchestration, beyond 
simple aggregation/batching use-cases.


## API composition checklist for a supergraph platform


<table>
  <thead>
    <tr>
      <td class="fixed-col-width-1"><b>Attribute</b></td>
      <td><b>Description</b></td>
    </tr>
  </thead>
    <tbody>
    <tr>
        <td><b>Integration</b></td>
        <td><i>Making it easy for API consumers to integrate APIs into their services</i></td>
    </tr>
    <tr>
        <td>Multiple API formats</td>
        <td>Can the supergraph platform to automatically provide output formats beyond GraphQL? Eg: REST/OpenAPI<br/> This is required to prevent a lock in to the GraphQL protocol as needs change over time.</td>
    </tr>
    <tr>
        <td>Documentation</td>
        <td>Does the supergraph platform help domain owners maintain documentation? If the underlying domain (database, code or APIs) are already documented, are those automatically picked up by the supergraph platform?</td>
    </tr>
    <tr>
        <td>Standardization</td>
        <td>Does the supergraph platform provide or enforce a standardized domain API design? (Eg: pagination, filtering, sorting etc) </td>
    </tr>
    <tr>
        <td><b>Aggregation</b></td>
        <td><i>Making it easy for API consumers to aggregate/batch multiple API calls into one</i></td>
    </tr>
    <tr>
        <td>Relationships</td>
        <td>Does the supergraph provide a way of creating relationships between any 2 entities or endpoints without requiring changes from the domain owners</td>
    </tr>
    <tr>
        <td>Composability</td>
        <td>How many "join" features does the supergraph provide, Given a relationship between 2 entities in the supergraph?</td>
    </tr>
    <tr>
        <td><b>Orchestration</b></td>
        <td>Making it easy for supergraph stakeholders to author custom API orchestration</td>
    </tr>
    <tr>
        <td>Federated mutations / decoupled orchestration business logic </td>
        <td>Does the supergraph provide a way to author orchestration flows within or across underlying domains? </td>
    </tr>
    </tbody>
  </table>

## Federated GraphQL anti-patterns

Building a federated GraphQL API with a supergraph is a strategic decision. 
When the key desired benefit of GraphQL is to solve an API composition problem on top of existing domains, then the key expected ROI is to improve API integration, aggregation and orchestration.
In this case, the following anti-patterns should be avoided. Building a federated GraphQL API with a supergraph is a strategic decision and the wrong choice can create thousands of person-hours of technical debt and legacy that become hard to unwind.

1. ❌ **Pure schema-driven design**: 
    - The situation: A schema driven in GraphQL is the approach of first building a schema that is entirely based on what consumers need. 
    - The problem: While this approach is important for the parts of the API that require API aggregation and custom API orchestration workflows, this approach negates the effort that has gone into and the effort that will continue to go into maintaining domain APIs. 
    - Symptoms: API producers are not willing to own their subgraphs and find it tedious to maintain another subgraph API. Additional subgraph teams and developers are required to maintain domain subgraphs.
    - The solution: Generating parts of the supergraph from the domain, and building parts of the supergraph based on consumer needs is the ideal approach. 
2. ❌ **Forcing subgraph owners to own inter-subgraph relationships**: 
    - The situation: Relationships between subgraphs can only be specified by subgraph owners.
    - The problem: While this approach allows subgraph owners to extend and connect their subgraphs to other subgraphs, it requires domain owners (subgraph owners) to understand other subgraphs.
    - Symptoms: Disconnected supergraphs. Inefficient query execution plans if relationships are hard to implement in some directions. API consumers who are frustrated at not being able to participate in creating inter-subgraph relationships. 
    - The solution: In addition to allowing subgraph owners to create inter-subgraph relationships, allow creation of relationships in the supergraph engine outside the subgraphs as well.
3. ❌ **Requiring an additional API registry to create custom resolvers (eg: federated mutations)**: 
    - The situation: Mutations can only originate from a domain subgraph.
    - The problem: Supergraph stakeholders are forced to create a new subgraph that directly connects to underlying domains. This requires stakeholders to refer to _another_ API registry to understand how to connect to the underlying subgraph domains (eg: database, REST).
    - Symptoms: API consumers are frustrated at not being able to easily create custom resolvers that represent some unique business logic & workflow for their specific need. Lack of an operating model around federating mutations, or API sagas or distributed transactions.
    - The solution: The supergraph platform should provide a way for supergraph stakeholders to author custom API workflows interspersed with business logic without having to refer to APIs outside of the supergraph.
