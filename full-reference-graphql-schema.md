```graphql

schema {
  query: query_root
  subscription: subscription_root
}

"""
Boolean expression to compare columns of type "Int". All fields are combined with logical 'AND'.
"""
input Int_comparison_exp {
  _eq: Int
  _gt: Int
  _gte: Int
  _in: [Int!]
  _is_null: Boolean
  _lt: Int
  _lte: Int
  _neq: Int
  _nin: [Int!]
}

"""
Boolean expression to compare columns of type "String". All fields are combined with logical 'AND'.
"""
input String_comparison_exp {
  _eq: String
  _gt: String
  _gte: String

  """does the column match the given case-insensitive pattern"""
  _ilike: String
  _in: [String!]

  """
  does the column match the given POSIX regular expression, case insensitive
  """
  _iregex: String
  _is_null: Boolean

  """does the column match the given pattern"""
  _like: String
  _lt: String
  _lte: String
  _neq: String

  """does the column NOT match the given case-insensitive pattern"""
  _nilike: String
  _nin: [String!]

  """
  does the column NOT match the given POSIX regular expression, case insensitive
  """
  _niregex: String

  """does the column NOT match the given pattern"""
  _nlike: String

  """
  does the column NOT match the given POSIX regular expression, case sensitive
  """
  _nregex: String

  """does the column NOT match the given SQL regular expression"""
  _nsimilar: String

  """
  does the column match the given POSIX regular expression, case sensitive
  """
  _regex: String

  """does the column match the given SQL regular expression"""
  _similar: String
}

"""
columns and relationships of "article"
"""
type article {
  """An object relationship"""
  author: author!
  author_id: Int!
  id: Int!
  publishDate: date

  """An array relationship"""
  ratings(
    """distinct select on columns"""
    distinct_on: [rating_select_column!]

    """limit the number of rows returned"""
    limit: Int

    """skip the first n rows. Use only with order_by"""
    offset: Int

    """sort the rows by one or more columns"""
    order_by: [rating_order_by!]

    """filter the rows returned"""
    where: rating_bool_exp
  ): [rating!]!

  """An aggregate relationship"""
  ratings_aggregate(
    """distinct select on columns"""
    distinct_on: [rating_select_column!]

    """limit the number of rows returned"""
    limit: Int

    """skip the first n rows. Use only with order_by"""
    offset: Int

    """sort the rows by one or more columns"""
    order_by: [rating_order_by!]

    """filter the rows returned"""
    where: rating_bool_exp
  ): rating_aggregate!
  title: String!
}

"""
aggregated selection of "article"
"""
type article_aggregate {
  aggregate: article_aggregate_fields
  nodes: [article!]!
}

input article_aggregate_bool_exp {
  count: article_aggregate_bool_exp_count
}

input article_aggregate_bool_exp_count {
  arguments: [article_select_column!]
  distinct: Boolean
  filter: article_bool_exp
  predicate: Int_comparison_exp!
}

"""
aggregate fields of "article"
"""
type article_aggregate_fields {
  avg: article_avg_fields
  count(columns: [article_select_column!], distinct: Boolean): Int!
  max: article_max_fields
  min: article_min_fields
  stddev: article_stddev_fields
  stddev_pop: article_stddev_pop_fields
  stddev_samp: article_stddev_samp_fields
  sum: article_sum_fields
  var_pop: article_var_pop_fields
  var_samp: article_var_samp_fields
  variance: article_variance_fields
}

"""
order by aggregate values of table "article"
"""
input article_aggregate_order_by {
  avg: article_avg_order_by
  count: order_by
  max: article_max_order_by
  min: article_min_order_by
  stddev: article_stddev_order_by
  stddev_pop: article_stddev_pop_order_by
  stddev_samp: article_stddev_samp_order_by
  sum: article_sum_order_by
  var_pop: article_var_pop_order_by
  var_samp: article_var_samp_order_by
  variance: article_variance_order_by
}

"""aggregate avg on columns"""
type article_avg_fields {
  author_id: Float
  id: Float
}

"""
order by avg() on columns of table "article"
"""
input article_avg_order_by {
  author_id: order_by
  id: order_by
}

"""
Boolean expression to filter rows from the table "article". All fields are combined with a logical 'AND'.
"""
input article_bool_exp {
  _and: [article_bool_exp!]
  _not: article_bool_exp
  _or: [article_bool_exp!]
  author: author_bool_exp
  author_id: Int_comparison_exp
  id: Int_comparison_exp
  publishDate: date_comparison_exp
  ratings: rating_bool_exp
  ratings_aggregate: rating_aggregate_bool_exp
  title: String_comparison_exp
}

"""aggregate max on columns"""
type article_max_fields {
  author_id: Int
  id: Int
  publishDate: date
  title: String
}

"""
order by max() on columns of table "article"
"""
input article_max_order_by {
  author_id: order_by
  id: order_by
  publishDate: order_by
  title: order_by
}

"""aggregate min on columns"""
type article_min_fields {
  author_id: Int
  id: Int
  publishDate: date
  title: String
}

"""
order by min() on columns of table "article"
"""
input article_min_order_by {
  author_id: order_by
  id: order_by
  publishDate: order_by
  title: order_by
}

"""Ordering options when selecting data from "article"."""
input article_order_by {
  author: author_order_by
  author_id: order_by
  id: order_by
  publishDate: order_by
  ratings_aggregate: rating_aggregate_order_by
  title: order_by
}

"""
select columns of table "article"
"""
enum article_select_column {
  """column name"""
  author_id

  """column name"""
  id

  """column name"""
  publishDate

  """column name"""
  title
}

"""aggregate stddev on columns"""
type article_stddev_fields {
  author_id: Float
  id: Float
}

"""
order by stddev() on columns of table "article"
"""
input article_stddev_order_by {
  author_id: order_by
  id: order_by
}

"""aggregate stddev_pop on columns"""
type article_stddev_pop_fields {
  author_id: Float
  id: Float
}

"""
order by stddev_pop() on columns of table "article"
"""
input article_stddev_pop_order_by {
  author_id: order_by
  id: order_by
}

"""aggregate stddev_samp on columns"""
type article_stddev_samp_fields {
  author_id: Float
  id: Float
}

"""
order by stddev_samp() on columns of table "article"
"""
input article_stddev_samp_order_by {
  author_id: order_by
  id: order_by
}

"""
Streaming cursor of the table "article"
"""
input article_stream_cursor_input {
  """Stream column input with initial value"""
  initial_value: article_stream_cursor_value_input!

  """cursor ordering"""
  ordering: cursor_ordering
}

"""Initial value of the column from where the streaming should start"""
input article_stream_cursor_value_input {
  author_id: Int
  id: Int
  publishDate: date
  title: String
}

"""aggregate sum on columns"""
type article_sum_fields {
  author_id: Int
  id: Int
}

"""
order by sum() on columns of table "article"
"""
input article_sum_order_by {
  author_id: order_by
  id: order_by
}

"""aggregate var_pop on columns"""
type article_var_pop_fields {
  author_id: Float
  id: Float
}

"""
order by var_pop() on columns of table "article"
"""
input article_var_pop_order_by {
  author_id: order_by
  id: order_by
}

"""aggregate var_samp on columns"""
type article_var_samp_fields {
  author_id: Float
  id: Float
}

"""
order by var_samp() on columns of table "article"
"""
input article_var_samp_order_by {
  author_id: order_by
  id: order_by
}

"""aggregate variance on columns"""
type article_variance_fields {
  author_id: Float
  id: Float
}

"""
order by variance() on columns of table "article"
"""
input article_variance_order_by {
  author_id: order_by
  id: order_by
}

"""
columns and relationships of "author"
"""
type author {
  age: numeric

  """An array relationship"""
  articles(
    """distinct select on columns"""
    distinct_on: [article_select_column!]

    """limit the number of rows returned"""
    limit: Int

    """skip the first n rows. Use only with order_by"""
    offset: Int

    """sort the rows by one or more columns"""
    order_by: [article_order_by!]

    """filter the rows returned"""
    where: article_bool_exp
  ): [article!]!

  """An aggregate relationship"""
  articles_aggregate(
    """distinct select on columns"""
    distinct_on: [article_select_column!]

    """limit the number of rows returned"""
    limit: Int

    """skip the first n rows. Use only with order_by"""
    offset: Int

    """sort the rows by one or more columns"""
    order_by: [article_order_by!]

    """filter the rows returned"""
    where: article_bool_exp
  ): article_aggregate!
  id: Int!
  name: String!
}

"""
aggregated selection of "author"
"""
type author_aggregate {
  aggregate: author_aggregate_fields
  nodes: [author!]!
}

"""
aggregate fields of "author"
"""
type author_aggregate_fields {
  avg: author_avg_fields
  count(columns: [author_select_column!], distinct: Boolean): Int!
  max: author_max_fields
  min: author_min_fields
  stddev: author_stddev_fields
  stddev_pop: author_stddev_pop_fields
  stddev_samp: author_stddev_samp_fields
  sum: author_sum_fields
  var_pop: author_var_pop_fields
  var_samp: author_var_samp_fields
  variance: author_variance_fields
}

"""aggregate avg on columns"""
type author_avg_fields {
  age: Float
  id: Float
}

"""
Boolean expression to filter rows from the table "author". All fields are combined with a logical 'AND'.
"""
input author_bool_exp {
  _and: [author_bool_exp!]
  _not: author_bool_exp
  _or: [author_bool_exp!]
  age: numeric_comparison_exp
  articles: article_bool_exp
  articles_aggregate: article_aggregate_bool_exp
  id: Int_comparison_exp
  name: String_comparison_exp
}

"""aggregate max on columns"""
type author_max_fields {
  age: numeric
  id: Int
  name: String
}

"""aggregate min on columns"""
type author_min_fields {
  age: numeric
  id: Int
  name: String
}

"""Ordering options when selecting data from "author"."""
input author_order_by {
  age: order_by
  articles_aggregate: article_aggregate_order_by
  id: order_by
  name: order_by
}

"""
select columns of table "author"
"""
enum author_select_column {
  """column name"""
  age

  """column name"""
  id

  """column name"""
  name
}

"""aggregate stddev on columns"""
type author_stddev_fields {
  age: Float
  id: Float
}

"""aggregate stddev_pop on columns"""
type author_stddev_pop_fields {
  age: Float
  id: Float
}

"""aggregate stddev_samp on columns"""
type author_stddev_samp_fields {
  age: Float
  id: Float
}

"""
Streaming cursor of the table "author"
"""
input author_stream_cursor_input {
  """Stream column input with initial value"""
  initial_value: author_stream_cursor_value_input!

  """cursor ordering"""
  ordering: cursor_ordering
}

"""Initial value of the column from where the streaming should start"""
input author_stream_cursor_value_input {
  age: numeric
  id: Int
  name: String
}

"""aggregate sum on columns"""
type author_sum_fields {
  age: numeric
  id: Int
}

"""aggregate var_pop on columns"""
type author_var_pop_fields {
  age: Float
  id: Float
}

"""aggregate var_samp on columns"""
type author_var_samp_fields {
  age: Float
  id: Float
}

"""aggregate variance on columns"""
type author_variance_fields {
  age: Float
  id: Float
}

"""ordering argument of a cursor"""
enum cursor_ordering {
  """ascending ordering of the cursor"""
  ASC

  """descending ordering of the cursor"""
  DESC
}

scalar date

"""
Boolean expression to compare columns of type "date". All fields are combined with logical 'AND'.
"""
input date_comparison_exp {
  _eq: date
  _gt: date
  _gte: date
  _in: [date!]
  _is_null: Boolean
  _lt: date
  _lte: date
  _neq: date
  _nin: [date!]
}

scalar numeric

"""
Boolean expression to compare columns of type "numeric". All fields are combined with logical 'AND'.
"""
input numeric_comparison_exp {
  _eq: numeric
  _gt: numeric
  _gte: numeric
  _in: [numeric!]
  _is_null: Boolean
  _lt: numeric
  _lte: numeric
  _neq: numeric
  _nin: [numeric!]
}

"""column ordering options"""
enum order_by {
  """in ascending order, nulls last"""
  asc

  """in ascending order, nulls first"""
  asc_nulls_first

  """in ascending order, nulls last"""
  asc_nulls_last

  """in descending order, nulls first"""
  desc

  """in descending order, nulls first"""
  desc_nulls_first

  """in descending order, nulls last"""
  desc_nulls_last
}

type query_root {
  """
  fetch data from the table: "article"
  """
  article(
    """distinct select on columns"""
    distinct_on: [article_select_column!]

    """limit the number of rows returned"""
    limit: Int

    """skip the first n rows. Use only with order_by"""
    offset: Int

    """sort the rows by one or more columns"""
    order_by: [article_order_by!]

    """filter the rows returned"""
    where: article_bool_exp
  ): [article!]!

  """
  fetch aggregated fields from the table: "article"
  """
  article_aggregate(
    """distinct select on columns"""
    distinct_on: [article_select_column!]

    """limit the number of rows returned"""
    limit: Int

    """skip the first n rows. Use only with order_by"""
    offset: Int

    """sort the rows by one or more columns"""
    order_by: [article_order_by!]

    """filter the rows returned"""
    where: article_bool_exp
  ): article_aggregate!

  """fetch data from the table: "article" using primary key columns"""
  article_by_pk(id: Int!): article

  """
  fetch data from the table: "author"
  """
  author(
    """distinct select on columns"""
    distinct_on: [author_select_column!]

    """limit the number of rows returned"""
    limit: Int

    """skip the first n rows. Use only with order_by"""
    offset: Int

    """sort the rows by one or more columns"""
    order_by: [author_order_by!]

    """filter the rows returned"""
    where: author_bool_exp
  ): [author!]!

  """
  fetch aggregated fields from the table: "author"
  """
  author_aggregate(
    """distinct select on columns"""
    distinct_on: [author_select_column!]

    """limit the number of rows returned"""
    limit: Int

    """skip the first n rows. Use only with order_by"""
    offset: Int

    """sort the rows by one or more columns"""
    order_by: [author_order_by!]

    """filter the rows returned"""
    where: author_bool_exp
  ): author_aggregate!

  """fetch data from the table: "author" using primary key columns"""
  author_by_pk(id: Int!): author

  """
  fetch data from the table: "rating"
  """
  rating(
    """distinct select on columns"""
    distinct_on: [rating_select_column!]

    """limit the number of rows returned"""
    limit: Int

    """skip the first n rows. Use only with order_by"""
    offset: Int

    """sort the rows by one or more columns"""
    order_by: [rating_order_by!]

    """filter the rows returned"""
    where: rating_bool_exp
  ): [rating!]!

  """
  fetch aggregated fields from the table: "rating"
  """
  rating_aggregate(
    """distinct select on columns"""
    distinct_on: [rating_select_column!]

    """limit the number of rows returned"""
    limit: Int

    """skip the first n rows. Use only with order_by"""
    offset: Int

    """sort the rows by one or more columns"""
    order_by: [rating_order_by!]

    """filter the rows returned"""
    where: rating_bool_exp
  ): rating_aggregate!

  """fetch data from the table: "rating" using primary key columns"""
  rating_by_pk(id: Int!): rating

  """
  execute function "search_authors" which returns "author"
  """
  search_authors(
    """
    input parameters for function "search_authors"
    """
    args: search_authors_args!

    """distinct select on columns"""
    distinct_on: [author_select_column!]

    """limit the number of rows returned"""
    limit: Int

    """skip the first n rows. Use only with order_by"""
    offset: Int

    """sort the rows by one or more columns"""
    order_by: [author_order_by!]

    """filter the rows returned"""
    where: author_bool_exp
  ): [author!]!

  """
  execute function "search_authors" and query aggregates on result of table type "author"
  """
  search_authors_aggregate(
    """
    input parameters for function "search_authors_aggregate"
    """
    args: search_authors_args!

    """distinct select on columns"""
    distinct_on: [author_select_column!]

    """limit the number of rows returned"""
    limit: Int

    """skip the first n rows. Use only with order_by"""
    offset: Int

    """sort the rows by one or more columns"""
    order_by: [author_order_by!]

    """filter the rows returned"""
    where: author_bool_exp
  ): author_aggregate!
}

"""
columns and relationships of "rating"
"""
type rating {
  """An object relationship"""
  article: article!
  article_id: Int!
  id: Int!
  stars: Int!
  user_id: Int!
}

"""
aggregated selection of "rating"
"""
type rating_aggregate {
  aggregate: rating_aggregate_fields
  nodes: [rating!]!
}

input rating_aggregate_bool_exp {
  count: rating_aggregate_bool_exp_count
}

input rating_aggregate_bool_exp_count {
  arguments: [rating_select_column!]
  distinct: Boolean
  filter: rating_bool_exp
  predicate: Int_comparison_exp!
}

"""
aggregate fields of "rating"
"""
type rating_aggregate_fields {
  avg: rating_avg_fields
  count(columns: [rating_select_column!], distinct: Boolean): Int!
  max: rating_max_fields
  min: rating_min_fields
  stddev: rating_stddev_fields
  stddev_pop: rating_stddev_pop_fields
  stddev_samp: rating_stddev_samp_fields
  sum: rating_sum_fields
  var_pop: rating_var_pop_fields
  var_samp: rating_var_samp_fields
  variance: rating_variance_fields
}

"""
order by aggregate values of table "rating"
"""
input rating_aggregate_order_by {
  avg: rating_avg_order_by
  count: order_by
  max: rating_max_order_by
  min: rating_min_order_by
  stddev: rating_stddev_order_by
  stddev_pop: rating_stddev_pop_order_by
  stddev_samp: rating_stddev_samp_order_by
  sum: rating_sum_order_by
  var_pop: rating_var_pop_order_by
  var_samp: rating_var_samp_order_by
  variance: rating_variance_order_by
}

"""aggregate avg on columns"""
type rating_avg_fields {
  article_id: Float
  id: Float
  stars: Float
  user_id: Float
}

"""
order by avg() on columns of table "rating"
"""
input rating_avg_order_by {
  article_id: order_by
  id: order_by
  stars: order_by
  user_id: order_by
}

"""
Boolean expression to filter rows from the table "rating". All fields are combined with a logical 'AND'.
"""
input rating_bool_exp {
  _and: [rating_bool_exp!]
  _not: rating_bool_exp
  _or: [rating_bool_exp!]
  article: article_bool_exp
  article_id: Int_comparison_exp
  id: Int_comparison_exp
  stars: Int_comparison_exp
  user_id: Int_comparison_exp
}

"""aggregate max on columns"""
type rating_max_fields {
  article_id: Int
  id: Int
  stars: Int
  user_id: Int
}

"""
order by max() on columns of table "rating"
"""
input rating_max_order_by {
  article_id: order_by
  id: order_by
  stars: order_by
  user_id: order_by
}

"""aggregate min on columns"""
type rating_min_fields {
  article_id: Int
  id: Int
  stars: Int
  user_id: Int
}

"""
order by min() on columns of table "rating"
"""
input rating_min_order_by {
  article_id: order_by
  id: order_by
  stars: order_by
  user_id: order_by
}

"""Ordering options when selecting data from "rating"."""
input rating_order_by {
  article: article_order_by
  article_id: order_by
  id: order_by
  stars: order_by
  user_id: order_by
}

"""
select columns of table "rating"
"""
enum rating_select_column {
  """column name"""
  article_id

  """column name"""
  id

  """column name"""
  stars

  """column name"""
  user_id
}

"""aggregate stddev on columns"""
type rating_stddev_fields {
  article_id: Float
  id: Float
  stars: Float
  user_id: Float
}

"""
order by stddev() on columns of table "rating"
"""
input rating_stddev_order_by {
  article_id: order_by
  id: order_by
  stars: order_by
  user_id: order_by
}

"""aggregate stddev_pop on columns"""
type rating_stddev_pop_fields {
  article_id: Float
  id: Float
  stars: Float
  user_id: Float
}

"""
order by stddev_pop() on columns of table "rating"
"""
input rating_stddev_pop_order_by {
  article_id: order_by
  id: order_by
  stars: order_by
  user_id: order_by
}

"""aggregate stddev_samp on columns"""
type rating_stddev_samp_fields {
  article_id: Float
  id: Float
  stars: Float
  user_id: Float
}

"""
order by stddev_samp() on columns of table "rating"
"""
input rating_stddev_samp_order_by {
  article_id: order_by
  id: order_by
  stars: order_by
  user_id: order_by
}

"""
Streaming cursor of the table "rating"
"""
input rating_stream_cursor_input {
  """Stream column input with initial value"""
  initial_value: rating_stream_cursor_value_input!

  """cursor ordering"""
  ordering: cursor_ordering
}

"""Initial value of the column from where the streaming should start"""
input rating_stream_cursor_value_input {
  article_id: Int
  id: Int
  stars: Int
  user_id: Int
}

"""aggregate sum on columns"""
type rating_sum_fields {
  article_id: Int
  id: Int
  stars: Int
  user_id: Int
}

"""
order by sum() on columns of table "rating"
"""
input rating_sum_order_by {
  article_id: order_by
  id: order_by
  stars: order_by
  user_id: order_by
}

"""aggregate var_pop on columns"""
type rating_var_pop_fields {
  article_id: Float
  id: Float
  stars: Float
  user_id: Float
}

"""
order by var_pop() on columns of table "rating"
"""
input rating_var_pop_order_by {
  article_id: order_by
  id: order_by
  stars: order_by
  user_id: order_by
}

"""aggregate var_samp on columns"""
type rating_var_samp_fields {
  article_id: Float
  id: Float
  stars: Float
  user_id: Float
}

"""
order by var_samp() on columns of table "rating"
"""
input rating_var_samp_order_by {
  article_id: order_by
  id: order_by
  stars: order_by
  user_id: order_by
}

"""aggregate variance on columns"""
type rating_variance_fields {
  article_id: Float
  id: Float
  stars: Float
  user_id: Float
}

"""
order by variance() on columns of table "rating"
"""
input rating_variance_order_by {
  article_id: order_by
  id: order_by
  stars: order_by
  user_id: order_by
}

input search_authors_args {
  search: String
}

type subscription_root {
  """
  fetch data from the table: "article"
  """
  article(
    """distinct select on columns"""
    distinct_on: [article_select_column!]

    """limit the number of rows returned"""
    limit: Int

    """skip the first n rows. Use only with order_by"""
    offset: Int

    """sort the rows by one or more columns"""
    order_by: [article_order_by!]

    """filter the rows returned"""
    where: article_bool_exp
  ): [article!]!

  """
  fetch aggregated fields from the table: "article"
  """
  article_aggregate(
    """distinct select on columns"""
    distinct_on: [article_select_column!]

    """limit the number of rows returned"""
    limit: Int

    """skip the first n rows. Use only with order_by"""
    offset: Int

    """sort the rows by one or more columns"""
    order_by: [article_order_by!]

    """filter the rows returned"""
    where: article_bool_exp
  ): article_aggregate!

  """fetch data from the table: "article" using primary key columns"""
  article_by_pk(id: Int!): article

  """
  fetch data from the table in a streaming manner: "article"
  """
  article_stream(
    """maximum number of rows returned in a single batch"""
    batch_size: Int!

    """cursor to stream the results returned by the query"""
    cursor: [article_stream_cursor_input]!

    """filter the rows returned"""
    where: article_bool_exp
  ): [article!]!

  """
  fetch data from the table: "author"
  """
  author(
    """distinct select on columns"""
    distinct_on: [author_select_column!]

    """limit the number of rows returned"""
    limit: Int

    """skip the first n rows. Use only with order_by"""
    offset: Int

    """sort the rows by one or more columns"""
    order_by: [author_order_by!]

    """filter the rows returned"""
    where: author_bool_exp
  ): [author!]!

  """
  fetch aggregated fields from the table: "author"
  """
  author_aggregate(
    """distinct select on columns"""
    distinct_on: [author_select_column!]

    """limit the number of rows returned"""
    limit: Int

    """skip the first n rows. Use only with order_by"""
    offset: Int

    """sort the rows by one or more columns"""
    order_by: [author_order_by!]

    """filter the rows returned"""
    where: author_bool_exp
  ): author_aggregate!

  """fetch data from the table: "author" using primary key columns"""
  author_by_pk(id: Int!): author

  """
  fetch data from the table in a streaming manner: "author"
  """
  author_stream(
    """maximum number of rows returned in a single batch"""
    batch_size: Int!

    """cursor to stream the results returned by the query"""
    cursor: [author_stream_cursor_input]!

    """filter the rows returned"""
    where: author_bool_exp
  ): [author!]!

  """
  fetch data from the table: "rating"
  """
  rating(
    """distinct select on columns"""
    distinct_on: [rating_select_column!]

    """limit the number of rows returned"""
    limit: Int

    """skip the first n rows. Use only with order_by"""
    offset: Int

    """sort the rows by one or more columns"""
    order_by: [rating_order_by!]

    """filter the rows returned"""
    where: rating_bool_exp
  ): [rating!]!

  """
  fetch aggregated fields from the table: "rating"
  """
  rating_aggregate(
    """distinct select on columns"""
    distinct_on: [rating_select_column!]

    """limit the number of rows returned"""
    limit: Int

    """skip the first n rows. Use only with order_by"""
    offset: Int

    """sort the rows by one or more columns"""
    order_by: [rating_order_by!]

    """filter the rows returned"""
    where: rating_bool_exp
  ): rating_aggregate!

  """fetch data from the table: "rating" using primary key columns"""
  rating_by_pk(id: Int!): rating

  """
  fetch data from the table in a streaming manner: "rating"
  """
  rating_stream(
    """maximum number of rows returned in a single batch"""
    batch_size: Int!

    """cursor to stream the results returned by the query"""
    cursor: [rating_stream_cursor_input]!

    """filter the rows returned"""
    where: rating_bool_exp
  ): [rating!]!

  """
  execute function "search_authors" which returns "author"
  """
  search_authors(
    """
    input parameters for function "search_authors"
    """
    args: search_authors_args!

    """distinct select on columns"""
    distinct_on: [author_select_column!]

    """limit the number of rows returned"""
    limit: Int

    """skip the first n rows. Use only with order_by"""
    offset: Int

    """sort the rows by one or more columns"""
    order_by: [author_order_by!]

    """filter the rows returned"""
    where: author_bool_exp
  ): [author!]!

  """
  execute function "search_authors" and query aggregates on result of table type "author"
  """
  search_authors_aggregate(
    """
    input parameters for function "search_authors_aggregate"
    """
    args: search_authors_args!

    """distinct select on columns"""
    distinct_on: [author_select_column!]

    """limit the number of rows returned"""
    limit: Int

    """skip the first n rows. Use only with order_by"""
    offset: Int

    """sort the rows by one or more columns"""
    order_by: [author_order_by!]

    """filter the rows returned"""
    where: author_bool_exp
  ): author_aggregate!
}

```
