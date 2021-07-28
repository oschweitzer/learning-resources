# GraphQL

- [GraphQL](#graphql)
  - [Definition](#definition)
  - [How it works](#how-it-works)
  - [Retrieve data](#retrieve-data)
  - [Add data](#add-data)
    - [Upload data](#upload-data)
  - [References](#references)

## Definition

GraphQL is a query language created by Facebook in 2012 and used as an alternative to REST for your API.

A GraphQL API is not so different from a REST API. It is a stateless, client-independent API used for exchanging data. But, unlike REST, GraphQL allows to perform much flexible queries.

A REST API has some limitations. For example, if you only want some piece of data, there are a two solutions, both with drawbacks.

- Write more endpoints. The issue with this solution is that you need to maintain a lot of endpoints and update them. If a frontend developer needs a new data, he's blocked until you add this new endpoint.
- Use query parameters. You only need one endpoint but your API wil become hard to understand because, it will be difficult to know which values can we set on these query parameters.

To solve this kind of issue, you can use GraphQL because it's like a query language for the backend where you can ask for specific data.

## How it works

In a GraphQL API, you only have one endpoint. You will use a POST request to tell the API the data you want. To do that, in the request body, you wil write a **Query Expression**.

A Query Expression is composed of,

- an **Operation type**: they are three operation types, `query` (to retrieve data), `mutation` (to manipulate data) and `subscription` (to set up realtime connection via WebSockets).
- an **Operation endpoint**: the available endpoints you defined.
- the **Requested fields**: the fields you want to extract. That is the flexible part of GraphQL.

A GraphQL basic workflow would be, a client sends a POST request with a Query Expression to a unique GraphQL endpoint. You defined your queries, mutations and subscriptions on your server. These elements are connected to resolvers, which are functions that contain your server logic. To compare this to REST, definitions are the routes and resolvers are the controllers.

## Retrieve data

Here is an example with Node.js and Express on how to retrieve data with GraphQL. You will need to install [Node.js](https://nodejs.org/en/) and [express](https://expressjs.com/fr/), [express-graphql](https://github.com/graphql/express-graphql) and [graphql](https://www.npmjs.com/package/graphql) npm packages.

**schema.js**

```javascript
const {buildSchema} = require ('graphql');

module.exports = buildSchema(`
  type TestData {
    text: String!
    views: Int!
  }
  
  type RootQuery {
    hello: TestData!
  }
  
  schema {
    query: RootQuery
  }`);
```

> The `!` character means that this property is non-nullable, GraphQL promises to always give you a value.

**resolvers.js**

```javascript
module.exports = {
  hello() {
    return {
      text: 'Hello World',
      views: 10
    };
  }
}
```

**app.js**

```javascript
const express = require('express');
const {graphqlHTTP} = require('express-graphql');

const graphqlSchema = require('./graphql/schema');
const graphqlResolver = require('./graphql/resolvers');

const app = express();

app.use('/graphql', graphqlHTTP({
  schema: graphqlSchema,
  rootValue: graphqlResolver,
  graphiql: true,
}));

app.listen(3000);
```

> By setting, `graphiql` to `true`, you can access http://localhost:3000/graphql on your web browser and have the [GraphiQL interface](https://github.com/graphql/graphiql) (a GraphQL IDE).

Then, you can use [Postman](https://www.postman.com/) to [send a GraphQL request](https://learning.postman.com/docs/postman/sending-api-requests/graphql/).

**Send an HTTP POST request with a GraphQL body**

```graphql
query {
    hello {
        text
        views
    }
}
```

## Add data

Here is an example that shows how to add data with GraphQL (add a user for example). What we need to do, is to create a mutation.

**schema.js**

```javascript
const {buildSchema} = require ('graphql');

module.exports = buildSchema(`

  type Post {
    _id: ID!
    title: String!
    content: String!
    author: User!
    createdAt: String!
    updatedAt: String!
  }

  type User {
    _id: ID!
    name: String!
    email: String!
    password: String
    status: String!
    posts: [Post!]!
  }

  input UserInputData {
    email: String!
    name: String!
    password: String!
  }
  
  type RootMutation {
    createUser(userInput: UserInputData): User!
  }
  
  schema {
    query: RootMutation
  }`);
```

> `input` is a special GraphQL keyword to use when you define a parameter schema.
> `ID` is a special GraphQL type, that means this property is unique.

**resolvers.js**

```javascript
module.exports = {
  createUser: async (
      {userInput}, // we can get the parameter object by destructuring or accessing via the 'args' object
      // (args.userInput)
      req) => {
    /*
      Put your logic here: check if user already exists, encrypt the password...
     */
    console.log(userInput);

    // Here we are simulating a database communication
    return await new Promise(resolve => setTimeout(() => {
      resolve({
         _id: 'zuoeghzrgzerrz74e5rze4r',
          name: userInput.name,
          email: userInput.email,
          password: '$2y$10$Xa3NwxsisVD3UFNdcPT2yO8EfGkjRzXTg0viYzeMfBloTz/DzGfLe',
          posts: []
      });
    }, 2000));

  }
}
```

Then, run a GraphQL request, using GraphiQL for example.

```graphql
mutation {
  createUser(userInput: {
    email: "john.doe@fakemail.com",
    name: "John Doe",
    password: "azerty"
  }) {
    _id
    name
    email
    password
  }
}
```

The response should be something like

```graphql
{
  "data": {
    "createUser": {
      "_id": "zuoeghzrgzerrz74e5rze4r",
      "name": "John Doe",
      "email": "john.doe@fakemail.com",
      "password": "$2y$10$Xa3NwxsisVD3UFNdcPT2yO8EfGkjRzXTg0viYzeMfBloTz/DzGfLe"
    }
  }
}
```

### Upload data

GraphQL only works with JSON data, but what if you want to upload a file, let's say an image to your server. To do that, it is recommended to have a REST API for the file upload and to reference this URL in your GraphQL request.

> Some GraphQL libraries allow to upload files ([Apollo](https://www.apollographql.com/) for example), but with vanilla GraphQL, use the above technique.

## References

- https://graphql.org/ : Official GraphQL website
- https://www.udemy.com/course/nodejs-the-complete-guide/learn/lecture/12197878#overview : Udemy Node.js course module on GraphQL