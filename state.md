# Dealing with state

State is how we can change the data inside of a component. In the former section we covered props, Props . Props are great but they lack the ability to be changed in the component they are added in. Let's look at the following example so you understand what I mean:

```
import React, { Component } from 'react';
import PropTypes from 'prop-types';

class Element extends React.Component {
  static propTypes = {
    name: PropTypes.string
  }

  // THIS WON'T WORK  
  changeName() {
    this.props.name = 'new name';
  }

  render() {
    return (
      <div>{this.props.name}</div>
      <button onClick={() => this.changeName()} ></button>
    )
  }
}

// usage

let person = { name: 'chris' }

<Element name={person.name} />
```

In the above example we try to change the name property but React won't let us do that. Instead we need to rely on state to do so. Let's rewrite the above to instead use state:

```
import React, { Component } from 'react';
import PropTypes from 'prop-types';

class Element extends React.Component {
  static propTypes = {
    name: PropTypes.string
  }
    
  constructor() {
    this.state = {
      name : this.props.name
    }
  }

  changeName() {
    this.setState({
      name: 'new name'
    })
  }

  render() {
    return (
      <div>{this.props.name}</div>
      <button onClick={() => this.changeName()} ></button>
    )
  }
}

// usage

let person = { name: 'chris' }

<Element name={person.name} />
```

Above we are now using the state functionality so we can change the value. We are however creating a copy of the props value in the constructor :

```
constructor() {
  this.state = {
    name : this.props.name
  }
}
```

We call the `setState()` method and provide it the slice of change we wan't to change here:

```
this.setState({
      name: 'new name'
    })

```

One important thing to know though. If the state object is way bigger than that, like so:

```
state = {
  name : '',
  products: []
}
```

Only the part you refer to in `setState()` will be affected.

## Listen to the change





