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

```js
<ThemeContext.Provider value='dark' >
  <ThemeContext.Consumer>
  {theme => <div>{theme}<div>} // outputs 'dark'
  </ThemeContextConsumer>
</ThemeContext>
```

Most likely the `ThemeContext.Consumer` is part of another component, like so:

```js
const ThemedButton = (props) => (
  <ThemeContext.Consumer>
  {theme => <button { ...props }>button with them: {theme}<button>} // outputs 'dark'
  </ThemeContextConsumer>
);

``` 
This leads to us being able to adjust our first code to:

```js
<ThemeContext.Provider value='dark' >
  <ThemedButton />
</ThemeContext>

```
As you can see the value from the provider is being passed down through the props and we can inside of the `ThemedButton` component access the `theme` property through the `Consumer`.

## Dynamic context
What if we want to change the provider value? One way of doing that is by having a dynamic context. We can achieve that by placing our Provider inside of a component and let its value depend on the state like so:

```js
class AnyComponent extends React.Component {
  state = {
    theme: 'dark'
  };

  render() {
    return (
      <ThemeContext.Provider value={this.state.theme}>
        <ThemedButton />
      </ThemeContext.Provider>
    );
  }
}
```

This will enable us to add further logic that let's us alter the theme anytime we want, like so:

```js
class AnyComponent extends React.Component {
  state = {
    theme: 'dark',
    themes: ['light', 'dark']
  };
  
  handleSelect = (evt) => {
    this.setState({
      theme: evt.target.value
    });
  };

  render() {
    return (
        <select value={this.state.theme} onChange=              {this.handleChangedTheme} >
          {this.state.themes.map(t => <option value={t}>{t}</option>)}
        </select>
        <div>
          Selected theme: {this.state.theme}
        </div>
      <ThemeContext.Provider value={this.state.theme}>
        <select>
        {this.themes.map(t => 
          <option onChange={this.handleSelect} value={t}>{t}</option>
        )}
        </select>
        <ThemedButton />
      </ThemeContext.Provider>
    );
  }
}

```

## Updating context from a component
## Higher order component

