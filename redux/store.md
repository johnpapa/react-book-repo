# Store
Well this all seems well and good but how does a reducer fit in with a having a global store of data? Well reducers are used as the access point to our global state. It's a way to protect the global state if you will. If we didn't use a reducer to manage our global state then potentially anything could change and we would quickly loose control. Let's show how we can let reducers handle our global state.

Imagine that we have a function `dispatch` that is our entry point for changing the global state. Let's sketch it out:

```js
const dispatch = (action) => {
}
```
Ok, so we take to inparameters `action`. The job of `dispatch` is to take an action and make sure it finds the correct reducer and have that reducer calculate a new state. What we do we mean by the right reducer? Well here is the thing. A reducer only operates on a slice of state, it never manages the entire state. There is usually one reducer for our property of our state object. Let us show this in code:

```js
{
  list: listReducer(state.list, action),
  user: userReducer(state.user, action)
}
```
As you can see above we can connect every property of our state object with a reducer function. Of course the above isn't valid code so what we need to do is to put this in a function, like so:

```js
const calc = (state, action) => {
  return {
    list: listReducer(state.list, action),
    user: userReducer(state.user, action)
  };
}
```
Now we can connect this to our `dispatch` function like so:

```js
const dispatch = (action) => {
  state = calc(state, action);
} 
```
As you can see above we also introduce a `state` variable that is nothing more than the state of our store. It looks like this:

```js
let state = {
  list: [],
  user: void 0
}
```
Our full code so far therefore looks like this:

```js
let state = {
  list: [],
  user: void 0
}

const calc = (state, action) => {
  return {
    list: listReducer(state.list, action),
    user: userReducer(state.user, action)
  };
}

const dispatch = (action) => {
  state = calc(state, action);
}
```

Now the above is the very engine in Redux. This is how we can send a message, have it processed via a reducer and finally the state changes accordingly. There are of course other things to this like the ability to select a slice of state and notify listeners. 


