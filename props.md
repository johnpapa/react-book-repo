# Introducing props

Adding props to your component means we add attributes to our component element. We can read what these attributes are and make them part of the markup, like so:

```
const jediData = {
  name: 'Yoda'
};

class Jedi extends React.Component {
  render() {
    return (
      <div>{this.props.jedi.name}</div>
    );
  }    
}

// example usage would be 

<Jedi jedi={jediData} />
```

In the above code we assign `jediData` to the attribute `jedi`. An attribute on component is known as a prop. To access it we simply need to type `this.props.jedi`. In the `render()` method above we type `this.props.jedi.name` and thereby we access the object we assigned to it and drill down to the `name` attribute.

## Assigning a list

## PropTypes

## Summary



