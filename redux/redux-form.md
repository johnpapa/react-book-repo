# Redux Form

`redux-form` is a library that makes it easier to handle forms and make them fit into the Redux system. Let's discuss briefly how we normally approach Forms without using this library. Our approach is usually this:

```js
state = {
  value: ''
}

onSubmit = (evt) => {
  // check the values, if ok, submit
}

onChange = (evt) => {
  this.setState({
    input: evt.target.value
  })
}

render() {
  return (
    <form onSubmit={this.onSubmit}>
      <div>
        <input onChange={this.onChange} value={this.state.value}>
      </div>
    </form>
}
```

Above we show a few things we need to do work with forms, which is

* keep track of when the value of inputs change, using `onChange`
* respond to a submit, using `onSubmit`

There are other things we want to keep track of in a form like:

- validating input, this is a big topic as it could be everything from checking wether an input is there at all or matching some pattern or may even need to check on server side wether something validates or not
- check if its pristine, untouched, if so no need to validate and definitely we shouldn't allow a submit to happen

Here is the good news, Redux Form library helps us with all these things, so let's show the awesomeness of this library in this chapter.

## Install and Set up
Installing it is as easy as typing:

```
yarn add redux-form
```
I'm assuming you already typed:

```
yarn add redux, react-redux
```
previously to get React working.

Setting it up is quite easy as well. What we need to do is to add some code in the file where we create the root reducer. I usually create a `store.js` file like so:

```js
// store.js

import { combineReducers } from 'redux'
import { reducer as formReducer } from 'redux-form'

const reducer = (state = 0, action) => {
  switch(action.type) {
    case 'INCREMENT':
      return state + 1;
    case 'DECREMENT':
      return state - 1;
    default:
      return state;
  }
}

const rootReducer = combineReducers({
  value: reducer,
})

export default rootReducer;
```
Above we have we a reducer function called `reducer` that we pass as a parameter to `combineReducers`. We create an association so that the function `reducer` takes care of `value` in the state of our store. To get `redux-form` to work we need to create the value `form` and let a form reducer take care of it like so:

```js
// store.js - excerpt
import { combineReducers } from 'redux'
import { reducer as formReducer } from 'redux-form'

// reducer definition

const rootReducer = combineReducers({
  value: reducer,
  form: formReducer,
})

export default rootReducer;
```
As you can see above we added the following line in our call to `combineReducers`:

```js
form: formReducer
```
and `formReducer` comes from `redux-form` and is something we import, like so:

```js
import { reducer as formReducer } from 'redux-form'
```



