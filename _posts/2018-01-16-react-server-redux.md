---
layout: post
title: Redux in react-server
---

I like Redux. The philosophy makes sense to me, I've had good luck in the past, and I'm doing everything I can to avoid getting swept up by new ideas everytime I try my hand at frontend development.

On react-server-redux
---
Of course I still have to learn how to set the damn thing up. The docs are again lacking, but the [well hidden examples](https://github.com/redfin/react-server/blob/master/packages/react-server-examples) might save me again. The [meteor-site](https://github.com/redfin/react-server/tree/master/packages/react-server-examples/meteor-site) example in particular has the full query+redux+render lifecycle.

It turns out that there's not too much beyond the normal Redux setup:

 * Set up the action methods, which may `dispatch(...)` other actions and then return either literal data or a thunk to resolve later.
 * Create a reducer to update state given some dispatched data.
 * Use `connect(...)` to wire the Redux `state` and `dispatch` into React components as `props`.
 * Create the Redux store during the `handleRoute` part of the `react-server` Page lifecycle.
 * Use `ReduxAdapter` and `RootProvider` from `react-server-redux` to defer React component processing based on the Redux store contents instead of a normal thunk,

Note that the Redux store does not know how to dispatch thunks by default. That means pulling in `thunkMiddleware` from `redux-thunk` and passing it to `createStore` when doing the `handleRoute` setup. The incantation is a bit longer than I expected:

```javascript
import { createStore, applyMiddleware, compose } from 'redux';
import thunkMiddleware from "redux-thunk";

const composeEnhancers = typeof window !== 'undefined' && window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ || compose;

var store = createStore(
  reducer,
  null,
  composeEnhancers(applyMiddleware(thunkMiddleware))
);
```
