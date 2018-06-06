# Reducer
A reducer is a function that is able to process our message, our [Action](/redux/actions.md). A reducer takes the existing state and applies the message on it. The end result is a new state. A reducer typically operates on a slice of state.

## Defining a reducer

A reducer typically looks like this:

```js
function reducer(state = [], action) {
  switch(action.type) {
    case 'CREATE_ITEM':
      return [ ...state, { ... action.payload}];
    case 'REMOVE_ITEM':
      return state.filter(item => item.id !== action.payload.id);
    default:
      return state;
  }
}
```



## Reducer Types
There are different types of re

## Slice of state