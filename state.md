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

```







