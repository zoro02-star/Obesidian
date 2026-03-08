>GraphQL vulnerabilities generally arise due to implementation and design flaws. For example, the introspection feature may be left active, enabling attackers to query the API in order to glean information about its schema.

>you first need to find its endpoint. As GraphQL APIs use the same endpoint for all requests, this is a valuable piece of information.

```
### Common endpoint names

GraphQL services often use similar endpoint suffixes. When testing for GraphQL endpoints, you should look to send universal queries to the following locations:

- `/graphql`
- `/api`
- `/api/graphql`
- `/graphql/api`
- `/graphql/graphql`
```
```
**Find the hidden GraphQL endpoint**
https://portswigger.net/web-security/graphql/lab-graphql-find-the-endpoint
```
```
bruteforce graphql 
https://portswigger.net/web-security/graphql/lab-graphql-brute-force-protection-bypass
```

```
in request use graphql like this
{
  "query": "query { getAllBlogPosts { id title author } getBlogPost(id: 3) { title author isPrivate postPassword } }"
}
```


