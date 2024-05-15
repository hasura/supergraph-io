# The Supergraph Manifesto

## Introduction

In data API design, a Supergraph is an API development methodology that offers reference specifications, design 
principles and an operating model. By implementing a Supergraph methodology, multiple teams in an organization can 
easily collaborate to deliver a modular, interconnected resource of data and logic and create a single, powerful, 
self-serve endpoint for API consumers.

## Table of Definitions
| Term               | Definition                                                                                                                     |
|--------------------|--------------------------------------------------------------------------------------------------------------------------------|
| **Supergraph**     | A Supergraph is an API creation methodology that offers reference specifications, design principles and an operating model.    |
| **supergraph API** | A modular, interconnected resource of data and logic as a single, powerful, self-serve API endpoint.                           |
| **Engine**         | Often referred to as a gateway or router in a GraphQL federation stack, managing API requests and responses.                   |
| **Data Domain**    | A distinct area of functionality or data.                                                                                      |
| **Subgraph**       | A modular component that acts as a self-contained entity within a larger supergraph and often represents a single data domain. |
| **API Consumer**   | An entity that uses APIs to access data and functionality.                                                                     |
| **API Producer**   | An entity that creates and maintains APIs.                                                                                     |

## Before the Supergraph
In conventional API design, data consumers need to manually access, integrate, and compose data from multiple 
endpoints to get the data they need. This is time-consuming, error-prone, and requires deep domain knowledge, often 
resulting in brittle integrations.

Infrastructure must also be built and maintained to support each of these data-access processes, and individual 
routes or resolvers built and maintained for each data access requirement.

![supergraphioERD.png](assets/supergraphioERD.png)

## Benefits
A Supergraph provides the following key benefits:

1. **Self-Serve API Consumer Composition**: Enables API integration, orchestration, and aggregation in a self-serve 
   manner.
2. **Federated Data Access Layer**: Provides a federated data layer that allows real-time access to data sources with
   cross-domain composability (joins, filtering, etc.).
3. **Incremental Adoption**: Offers a stable API that supports zero-downtime and incremental adoption.

## Supergraph Lifecycle

![ConnectConsumeDiscoverERD.png](assets/ConnectConsumeDiscoverERD.png)

### Connect Domains
As an API producer, you should easily and seamlessly connect your data domains to the platform. The Supergraph strategy 
and architecture addresses common challenges and reluctance from domain owners by ensuring:

1. **Subgraph Connector CI/CD**: Keeps the API contract in sync with minimal overhead for the domain owner.
2. **Subgraph Connector Performance**: Maintains or improves performance compared to direct access to the underlying 
   domain.

### Consume APIs
API consumers should be able to discover and consume APIs without manual integration, aggregation, or composition 
efforts as far as possible. Key requirements include:

1. **Composable API Design**: Allows on-demand composability, often utilizing GraphQL for expressive API design.
2. **API Portal**: Provides high-quality search, discovery, and documentation to facilitate self-serve consumption.

### Discover Demand
Understanding how API consumers use domains and identifying their unmet needs is crucial for API producers. The 
Supergraph platform supports:

1. **API Consumption Analytics**: Provides insights to improve the API marketplace for consumers.
2. **Ecosystem Integrations**: Integrates with existing tools to understand unmet demand and enhance the ecosystem.

### A Supergraph creates a virtuous cycle of success for API producers and consumers.

## Supergraph Architecture

### Control Plane (CI/CD and Build System)

The control plane ensures seamless connection of data domains to the Supergraph.

There are three components in the control plane of the Supergraph:

#### 1. The data domain itself
A database, API or lambda function that provides the data.
   
#### 2. The subgraph
API models, documentation, relationships, authorization policies. 

#### 3. The Supergraph
Centralized auth, governance, API conventions.

### Control Plane Lifecycle

![control-plane-lifecycle.png](assets/control-plane-lifecycle.png)

### Distributed Data Plane
The data plane ensures high-performance access to upstream domains, maintaining domain performance without hidden 
future maintenance costs.

## API Schema Design Guide

### Standardization
A Supergraph API schema should standardize:

1. **Models & Commands**: Separating resources (data collections) and methods (business logic).
2. **Filtering, Sorting, Pagination, Aggregation**: Providing consistent capabilities across models.

### Composability
The Supergraph API offers varying degrees of composability, including:

1. **Joining Data**: Combining related data.
2. **Nested Filtering, Sorting, Pagination, Aggregation**: Advanced queries involving related entities.

## Use Cases and Scenarios
Explore common scenarios and detailed use cases to understand the practical applications of the Supergraph architecture.

## Summary
The Supergraph architecture transforms API production and consumption by providing a unified, self-serve platform that 
simplifies data access and integration, enabling API producers and consumers to thrive in a connected ecosystem.

## More Reading
- [Use Cases](/use-cases)
- [Reference API Schema](/reference-api-schema)
- [FAQ](/faq)