---
title: Webpack loader
---

You can load GraphQL queries over `.graphql` files using Webpack. The package `graphql-tag` comes with a loader easy to setup and with some benefits:

1. Do not process GraphQL ASTs on client-side
2. Enable queries to be separated from logic

In the example below, we create a new file called `currentUser.graphql`:

```graphql
query CurrentUserForLayout {
  currentUser {
    login
    avatar_url
  }
}
```

You can load this file adding a rule in your webpack config file:

```js
module: {
  rules: [
    {
      test: /\.(graphql|gql)$/,
      exclude: /node_modules/,
      loader: 'graphql-tag/loader',
    },
  ],
},
```

As you can see, `.graphql` or `.gql` files will be parsed whenever imported:

```js
import React, { Component } from 'react';
import { graphql } from 'react-apollo';
import currentUserQuery from './currentUser.graphql';

class Profile extends Component { ... }
Profile.propTypes = { ... };

export default graphql(currentUserQuery)(Profile)
```

## Jest

[Jest](https://facebook.github.io/jest/) can't use the Webpack loaders. To make the same transformation work in Jest, use [jest-transform-graphql](https://github.com/remind101/jest-transform-graphql).

## Fragments

You can use and include fragments in .graphql files and have webpack include those dependencies for you, similar to how you would use fragments and queries with the gql tag in plain JS.


```graphql
#import "./UserInfoFragment.graphql"

query CurrentUserForLayout {
  currentUser {
    ...UserInfo
  }
}
```

See how we import the UserInfo fragment from another .graphql file (same way you'd import modules in JS).

And here's an example of defining the fragment in another .graphql file.

```graphql
fragment UserInfo on User {
  login
  avatar_url
}
```
