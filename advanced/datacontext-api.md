# Data context

What is a context. The React Context exist so you don't have to pass in data manually at every level. Context is about sharing data to many components. Therefore things that belong in a context is data that is considered global like a `user` or a `cart` etc.

Popular things that is usually a context are cross cutting data like:

- theme
- cart
- user

Don't use it for everything.

## Creating a context
You create a context by typing the following:

```js
const ThemeContext = React.createContext('light');
```

You need to do two things to be able to use it:

- declare a provider, this will be an outer element where you supply the data
- declare a consumer, this will be an inner element where you are able to interact with the provided data and display it or in some other way let it affect your application

The above context declares a provider by typing:

```
<ThemeContext.Provider value='dark' >
  <ThemeContext.Consumer>
  {theme => <div>{theme}<div>} // outputs 'dark'
  </ThemeContextConsumer>
</ThemeContext>
```


### Provider
### Consumer
## Dynamic context
## Updating context from a component
## Higher order component

