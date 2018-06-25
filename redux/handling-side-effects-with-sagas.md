# Handling side effects with Sagas
So far when we have built our app using Redux we have worked in synchronous data. Everything we do happens straight away wether we are incrementing a variable, adding an item to a list and so on. A real app will most likely do asynchronous work, performing AJAX requests when fetching and changing data. How would that look if we weren't using Sagas:

```js
// some container component

const createAction = () => ({ type: 'CREATED'  });

const loadAction = (data) => ({ type: 'LOAD_DATA', payload: data  });


const mapDispatchToProps = (dispatch) => {
  return {
    add: (data) => {
      const response = await fetch('url', {  
        method: 'POST',
        body: JSON.stringify(data)
      });
      const responseData = response.json();
      dispatch(createAction());
    },
    get: () => {
      const response = await fetch('url');
      const responseData = response.json();
      dispatch(loadAction(responseData));
    }
  }
}
```
Ok, so above we see that we are defining a `mapDispatchToProps` and creates two properties on the object we return back, `add` and `get`. Now what happens inside them is that we use some way to perform an AJAX call and when done we talk to Redux and our store by calling `dispatch`. 

We can definitely clean up the above a little so it doesn't look so bad but the fact remains we are mixing AJAX communication with Redux. There is a way to handle this a bit more elegantly namely by using Sagas. 

So Sagas promises to create a bit more order. The idea with Sagas is that the only thing you should see in a container component are dispatching of actions. Sagas acts as listeners to specific actions. When a specific action happens the Saga will have the ability to intervene, do its AJAX interaction and then end it all with dispatching an action. 

Sounds simple enough right? Let's get started.

## Installation & Setup

To use Sagas we need to install it by typing:

```js
yarn add redux-saga
``` 

Next step is about telling Redux we want to use Sagas. Sagas are a so called middleware, something that intercepts the normal Redux flow which means we need to do the following to make Sagas work:

- import and register Sagas as a middleware
- create a saga
- run a saga


