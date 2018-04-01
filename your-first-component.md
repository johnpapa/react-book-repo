# Your first component

There are many ways to create a component but let's have a look at how you can create a component. We use JSX to define it and assign it to a variable. Thereafter we are free to use it in our markup. To create our component we need the following:

* We need to inherit from React.Component
* We need to implement a render\(\) method that renders out in JSX what the component should look like

Let's begin by creating a Jedi component:

```js
class Jedi extends React.Component {
  render() {
    return (
      <div>I am a Jedi Component</div>
    );
  }  
}

// usage in markup

<div>
  <Jedi />
</div>
```

Ok, great I know how to create a Component but how do I actually create a React app? That's a fair comment. In the previous chapter[ Set up](/set-up.md) we show three different ways to help you get started, unpkg, create-react-app and setting up your own project with Webpack. For now let's stick to the simpler version with unpkg. For this we will need the following files:

* index.html
* app.js

```
// index.html

<html>
  <body>
    <!-- This is where our app will live  -->
    <div id="app"></div>
    
    <!-- These are script tags we need for React, JSX and ES2015 features  -->
    <script src="https://fb.me/react-15.0.0.js"></script>
    <script src="https://fb.me/react-dom-15.0.0.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/babel-core/5.8.34/browser.min.js"></script>
    <script type="text/babel" src="app.js"></script>
  </body>

</html>
```

and app.js looks like the following:

```js
class Jedi extends React.Component {
  render() {
    return (
      <div>I am a Jedi Component</div>
    );
  }
}

class Application extends React.Component {
  render() {
    return (
      <div>
        <Jedi />
      </div>
    );
  }
}

ReactDOM.render(<Application />, document.getElementById('app'));

```



