# GraphQL

## GraphQL vs. REST

### REST
1. Different URL endpoints (/authors, /authors/1, /authors/2)
2. HTTP verbs (GET, POST...)
3. Return all the data for a specific combination of a URL and an HTTP verb.
4. If we need /authors/1 and /books/10 we need to make 2 requests. n+1 queries problem.
5. No need of a library.
6. Cache GET request.

### GraphQL
1. One endpoint.
2. One verb (POST)
3. Return only the data we request.
4. Can get additional data in one request, but also has n+1 problem related to extra database requests.
5. Need a library.
6. Caching is difficult.

#### N+1 problem

```graphql
type Author {
  id: ID!
  name: String!
  books: [Book!]!
}

type Book {
  id: ID!
  title: String!
  author: Author!
}
```

```graphql
query {
  authors {
    name
    books {
      title
    }
  }
}
```

To resolve this query, the server might first fetch all the authors (1 query), and then for each author, fetch their related books (N queries, where N is the number of authors). This results in a total of N+1 queries to the data source.

To solve the N+1 problem, GraphQL server implementations often use techniques like "batching" and "caching". One popular approach is to use a DataLoader library, which groups multiple requests for related data into a single batch request, reducing the number of round trips to the data source and improving performance.
