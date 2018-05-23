# Forms II - validation
Validating forms can be done in different ways:
- Validate an element as soon as you type, and immediately indicate any error
- Validate the form on pressing submit

## Validate an element
You can have all the elements and their validation in one giant component but it is usually better to create a dedicated component for this. A component like this should have the following:

- render the element itself, this will give us more control over how the element is rendered
- an element where you can output the validation error
- a way to listen to changes and validate itself while the user types
- a way to indicate to the form that it is no longer valid should a validation error occur

### Render the element

With this one we mean that we need to create a component around our element. To do so is quite simple:

```
import React from 'react';

class Input extends React.Component {
 render() {
   return (
     <input { ...this.props} /> 
   );
 }
}
```
As you can see above we make the `Input` component render an `input` element tag. We also ensure that all properties set on the component make it to the underlying input by typing `{ ...this.props}`. A more manual version of the last one would be to type:

```
<input title={this.props.title} value={this.props.value} ... >
```
Depending on how many attributes we want to send to the underlying input, it could be quite a lot of typing. 

As we are now in control of how the `input` is rendered we can do all sorts of things like adding a `div` element, give it padding, margin, borders etc. Best part is we can reuse this component in a lot of places and all of our inputs will look nice and consistent.

### Adding validation

Now we will see that it pays off to wrap our `input` element in a component. Adding validation to our element is as easy as:

- add element placeholder where your error should be shown
- add a function that validates the input value
- validate on every value change, we need to add a callback to `onChange`

Let alter the `render()` method to the below:

```
render() {
  return (
    <InputContainer>
      {this.state.error &&
        <ErrorMessage>{this.state.error}</ErrorMessage>
      }
      <div>
        {this.props.desc}
      </div>
      <InnerInput value={this.state.data} onChange={this.handleChange} {...this.props} />
    </InputContainer>
  );
}
```

Here we are conditionally displaying the error message, assuming its on the state:

```
{this.state.error &&
  <ErrorMessage>{this.state.error}</ErrorMessage>
}
```
We are also hooking up a `handleChange()` method to `onChange`. Next step is adding our validation function:

```
const validate = (val, errMessage) => {
  const valid = /^[0-9]{2,3}$/.test(val);
  return valid ? '' : errMessage;
};
``` 
Our function above simply tests wether our input value matches a RegEx pattern and if so its valid, if not then we return the error message. So who is calling this function? Well the handleChange() method is, like so:

```
handleChange = (ev) => {
  const error = validate(ev.target.value, this.props.errMessage);
  this.setState({
    data: ev.target.value,
    error,
  });
}
```

We do two things here, firstly we call `validate()` to see if there was an error and secondly we set the state, which is our value and the error. If the error is an empty string then it is counted as `falsy`. So we can always safely set the error property and any error message would only be visible when it should be visible.

The full code for our component so far looks like this:

```
import React from 'react';
import styled from 'styled-components';
import PropTypes from 'prop-types';

const InnerInput = styled.input`
  font-size: 20px;
`;

const InputContainer = styled.div`
  padding: 20px;
  border: solid 1px grey;
`;

const ErrorMessage = styled.div`
  padding: 20px;
  border: red;
  background: pink;
  color: white;
`;

const validate = (val, errMessage) => {
  const valid = /^[0-9]{2,3}$/.test(val);
  return valid ? '' : errMessage;
};

class Input extends React.Component {
  static propTypes = {
    name: PropTypes.string,
    desc: PropTypes.string,
    errMessage: PropTypes.string,
  };

  state = {
    error: '',
    data: '',
  }

  handleChange = (ev) => {
    const { errMessage, name } = this.props;

    const error = validate(ev.target.value, errMessage);
    this.setState({
      data: ev.target.value,
      error,
    });
  }

  render() {
    return (
      <InputContainer>
        {this.state.error &&
          <ErrorMessage>{this.state.error}</ErrorMessage>
        }
        <div>
          {this.props.desc}
        </div>
        <InnerInput value={this.state.data} onChange={this.handleChange} {...this.props} />
      </InputContainer>
    );
  }
}

export default Input;

```