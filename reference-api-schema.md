The following is an excerpt from a complete reference GraphQL schema (*linked at the end*) that:
  - is built on 3 data domains `author` (which includes a function `search_author` that returns `author` search results), `article` and `rating`
  - implements all standardization attributes (S1-S5) by implementing filtering, sorting, pagination, aggregations for the models' collections as well as implementing a command for the `search` function
  - implements all composability attributes (C1-C5) by leveraging the following relationships between the 3 domains:
      - `author` <> `article` have a `1:many` relationship
      - `article` <> `rating` have a `1:many` relationship (*many users can leave a rating for the same article*)

<table>
<tr>
<td><b>Principle/Feature</b></td> <td><b>Reference type definition</b></td> <td><b>Example type definition</b></td> <td><b>Example query</b></td>
</tr>

<tr>
<td>
    Model type definition
</td> 
<td>

```
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
    <b>Models collection</b> (<i>support for filtering, pagination, sorting and limits<i>)
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
    <b>Model singleton</b>
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

[Complete reference GraphQL schema](/complete-reference-graphql-schema.graphql)
