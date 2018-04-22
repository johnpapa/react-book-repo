# Conditional rendering

Conditional rendering is about deciding what to render given a value of a variable. There are different approaches we can have here:

* render if true
* ternary expression

## Render if true

Let's have a look at the first version:

```
class Element extends React.Component {

  state = {
    show: false
  };

  const toggle = () => {
    this.setState({
      show: this.state.show
    });
  }

  render() {
    <div>some data</div>
    { this.state.show && 
      <div>body content</div>
    }
    <button onClick={this.toggle}></button>
  }
} 
```

Above we can see that look at the variable show in our state and renders a div if show is truthy:

```
{ this.state.show && 
      <div>body content</div>
}
```

## Ternary rendering

In this version we define a ternary expression and render different things depending on the value of our variable:



