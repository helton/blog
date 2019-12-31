---
title: Redux
date: "2019-12-29T12:39:00.000Z"
description: "The simplicity and power of this state management library"
tags: ["redux", "javascript", "library", "guide"]
---

Once upon a time there was a couple of engineers at Facebook who weren't happy about their apps state control. The behaviour was unpredictable and it was hard do add new features to the codebase. So, they made Flux.

Some time later, Dan Abramov borrowed their ideas and made a better library: **Redux**.

## Setup

### Installing Redux (via yarn):

`$ yarn add redux`

### Installing Redux (via npm):

`$ npm install redux --save`

### Using CDN

If you want to use it directly on your view (html page), just put the redux.js (or redux.js.min) in your `<script>` tag.
There a few CDNs availables. Just some of them:
- https://cdnjs.cloudflare.com/ajax/libs/redux/4.0.5/redux.min.js
- https://cdn.jsdelivr.net/npm/redux@4.0.5/lib/redux.min.js

## Concepts

Redux is based in the 3 following principles:

### a) A single immutable state (*one state to rule them all!*)

Everything that changes in your application, including the data and the UI state, is contained in a single JavaScript object called **state** or **state tree**.

In Redux all mutations are explicit and doing so we're able keep track of them over time.

### b) The state is read-only (*you can't touch this*)

Anytime you want to modify the state you need to dispatch an action. An action is a plain JavaScript object that represents the changes of the data.

The structure of the action object is up to you, the only requirement is that it should have a **type** property (frequently a String), which identify what the action is supposed to change.

Don't matter where the information comes from, it'll be added to the application through actions. This allows us to keep a fine control over our application changes, making its behaviour predictable.

Remember to put only the minimal amount of information into the action object, just enough to identify what changed and what will be updated in the **state** object (by the **reducers** functions).

Example:

```javascript
let action = { 
    type: "CHANGE_NAME",
    data: { 
        id: 0,
        name: 'Helton'
    }
}
```
This approach scales well to medium and large applications.

### c) Reducers handle state mutation through actions ((state, action) => new state!)

Reducers are functions that take the previous **state** of the application and the **action** being dispatched and returns the next **state**. It can't modify the current state, it should return a brand new object (the function should be pure). And no, it won't be slow!

For example:

```javascript
const counter = (state = 0, action = { }) => {
  switch (action.type) {
    case 'INCREMENT':
      return state + 1;
    case 'DECREMENT':
      return state - 1;
    default:
      return state;
  }
};
```

## The *store*

The store binds together the three principles of Redux. It holds the application's state object and lets you dispatch actions.

When you create it, you specify a reducer function.

In order to use store we need:

- Importing the _createStore _function:

```javascript
import { createStore } from 'redux';
```

- Create a local store passing as argument the reducer previously created:
```javascript
const store = createStore(counter);
```

## References

If you want more information, don't forget to check out the awesome resources below. It's worth it, trust me!

### Documentation

- [Redux official website](https://redux.js.org/)

### Courses

- [Egghead.io - Getting Started with Redux [by Dan Abramov]](https://egghead.io/courses/getting-started-with-redux)
- [Egghead.io - Building React Applications with Idiomatic Redux [by Dan Abramov]](https://egghead.io/courses/building-react-applications-with-idiomatic-redux)

### Awesome Lists

- [awesome-redux: Awesome list of Redux examples and middlewares](https://github.com/xgrommx/awesome-redux)

### Cheatsheets

- [Awesome Redux cheatsheet, by Rico Sta. Cruz](https://devhints.io/awesome-redux)