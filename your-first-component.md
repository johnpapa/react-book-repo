# Your first component

There are many ways to create a component but let's have a look at how you can create a component. We use JSX to define it and assign it to a variable. Thereafter we are free to use it in our markup. To create our component we need the following:

* We need to inherit from React.Component
* We need to implement a render\(\) method that renders out in JSX what the component should look like

Let's begin by creating a Jedi component:

```js
class Jedi extends React.Component {
  constructor() {}
  
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



