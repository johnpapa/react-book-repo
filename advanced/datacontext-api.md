# Data context

What is a context. The React Context exist so you don't have to pass in data manually at every level. Context is about sharing data to many components. Therefore things that belong in a context is data that is considered global like a `user` or a `cart` etc.

Popular things that is usually a context are cross cutting data like:

- theme
- cart
- user

Don't use it for everything.

## Creating a context
You create a context by typing the following:

```
const ThemeContext = React.createContext('light');
```


### Provider
### Consumer
## Dynamic context
## Updating context from a component
## Higher order component

