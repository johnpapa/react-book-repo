# Redux for React

* defining the store
* define presentation components
* define container components
* CRUD

## Get started

We need to install a couple of dependencies for this:

```
yarn add redux react-redux
```

Once we have those we are ready to begin

## Adding Redux to React

We need to do the following to make it work:

* create a store
* expose the store with a Provider
* create a container component

### Creating a store

Creating a store is about creating the needed reducers use a few helper functions to tell Redux about it. Let's have a look at what creating the reducers might look like:

```js
// store.js
import { combineReducers } from 'redux';

const listReducer = (state = [], action) => {
  switch(action.type) {
    case 'CREATE_ITEM':
      return [ ...state, { ...action.payload }];
    case 'REMOVE_ITEM':
      return state.filter(item => item.id !== action.payload.id);
    default:
      return state;
  }
};

const store = combineReducers({
  list: listReducer
});

export default store;
```

Above we have only defined one reducer, in the same file as `store.js` no less. Normally we would define one reducer per file and have them imported into `store.js`. Now we have everything defined in one file to make it easy to understand what is going on. One thing worth noting is our usage of the helper function `combineReducers`. This is the equivalent of writing:

```js
calc = (state, action) => {
  return {
    list: listReducer(state.list, action)
  }
}
```

It's not an exact match but it is pretty much what goes on internally in `combineReducers`.

### Expose the store via a provider

Next step is to wire everything up so we need to go to our `index.js` file and import our store expose it using a provider. We need to perform the following steps:
- import `createStore` and invoke it to create a store instance
- add the store to a Provider

First thing we do is therefore:

```js
// index.js - excerpt
import { createStore } from 'redux';
import app from './store';

const store = createStore(app);
```
Next step is to wrap our root component `App` in a `Provider` and set its `store` property to our newly created store, like so:

```js
// index.js - excerpt

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
document.getElementById('root'));
```


#### Add initial state
We can give our store an initial value, maybe there is some starter data that our app needs, we can do so by calling `dispatch` on our store instance like so:

```js
// index.js - excerpt

store.dispatch({ type: 'CREATE_ITEM', payload: { title: 'first item' } });
```

#### Full code
The full code with all the needed imports and calls looks like this:

```js
// index.js

import React from 'react';
import { Provider } from 'react-redux';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import registerServiceWorker from './registerServiceWorker';
import { createStore } from 'redux'
import app from './store';

const store = createStore(app)
store.dispatch({ type: 'CREATE_ITEM', payload: { title: 'first item' } });

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
document.getElementById('root'));
registerServiceWorker();
```
Let's split



