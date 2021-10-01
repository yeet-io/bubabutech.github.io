+++
title="GraphQL, TypeDI, Data Loaders, and Scoped Containers"
date=2021-10-01

[taxonomies]
categories = ["coding"]

[extra]
author = "Abner Qian"
+++

While working on a project for a client, we came across an interesting twist to a common problem. We were tasked with making the API backend that powers a mobile app called [Playhouse](https://www.playhouse.so/), a mobile app that displays real estate listings to its users in a nice little video package. We decided to make a [GraphQL](https://graphql.org/) API and got to building. During the design process, we made a point of keeping it maintainable for the long run.

<!-- more -->

## Application Architecture

This backend is a [Node.js](https://nodejs.org/en/) application with a [PostgreSQL](https://www.postgresql.org/) database. Since we were also using [TypeScript](https://www.typescriptlang.org/), we decided to use [TypeGraphQL](https://typegraphql.com/) to handle our GraphQL needs. Finally, one of the steps we took to improve maintainability was to use the dependency injection design pattern with the help of [TypeDI](https://github.com/typestack/typedi).

## The N+1 Problem

Those who have worked will GraphQL will likely be familiar with this problem already. When implementing a resolver for an object, if you just hit the database as soon as the resolver is called, you'll end up querying the database more than neccesary - sometimes a *lot* more than necessary.

Let's take a look at an example. In our API, we have listings, comments, and users. A listing can have multiple comments, and the comments are authored by users. When looking up a listing, its comments, and the comments' authors, the GraphQL query would look something like this:

```graphql
query {
  listing(id: 123) {
    id
    # ... a bunch of other fields
    comments {
      id
      body
      author {
        id
        username
      }
    }
  }
}
```

Let's take a look at what would happen if your comment's author resolver is written naively and looks up the user immediately.

```typescript
@Resolver(() => Comment)
export class CommentResolver {

  // ... other resolvers

  @FieldResolver(() => User)
  async author(
    @Root() { authorId }: Comment,
  ): Promise<User> {
    const user = await db('users').where({id: authorId})[0]
    return toSchema(user, User)
  }
}
```

In our application, we have a debug mode that outputs all the SQL queries that were used for every query that was run. This is what it outputs:

```typescript
{
  bindings: [ 123 ],
  sql: 'select * from "listings" where "id" = ?'
}
{
  bindings: [ 123 ],
  sql: 'select * from "comments" where "listing_id" = ?'
}
{
  bindings: [ 5 ],
  sql: 'select * from "users" where "id" = ?'
}
{
  bindings: [ 12 ],
  sql: 'select * from "users" where "id" = ?'
}
{
  bindings: [ 1 ],
  sql: 'select * from "users" where "id" = ?'
}
{
  bindings: [ 19 ],
  sql: 'select * from "users" where "id" = ?'
}
```

As you can see, there are four identical select statements on the users table where only the bindings change. This is because there are four comments on the listing and each comment calls the author resolver and makes its own database query. As the number of comments increases, the number of database queries would also increase. This is a huge performance issue.

## Data Loaders

Talk about how data loaders solve the N+1 problem.

## New Problems

Talk about how data loader are not supposed to persist between requests so they could not become TypeDI services. There were some janky solutions we used such as putting all the loaders into the context, but it was not a very good solution.

## Scoped Containers

Talk about scoped containers in TypeDI and why they were the solution.
