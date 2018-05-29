# Programmatic navigation

Usually we navigate using the `Link` component but sometimes we need to navigate based on an action. Then we are talking about programmatic navigation. There are essentially two ways of making that happen:

- history.push('url')
- Redirect component

## Using history
To use the first one we need to inject the `history` inside of our component. This is accomplished by using `withRouter()` function. Below is an example of a component using the described:

```js
import React from 'react';
import { withRouter } from 'react-router-dom';

class TestComponent extends React.Component {
  navigate= () => {
    this.props.history.push('/');
  }    
  
  render() {
    return (
      <React.Fragment>
        <button onClick={this.navigate}>Navigate</button>
      </React.Fragment>
    );
  }
}

export default withRouter(TestComponent);
```