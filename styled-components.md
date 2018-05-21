# Styled components

Styled components is an awesome library to enable you to:

* create reusable styles
* enables you to write actual CSS in your javascript

and much much more.

Let's install this library and see what you can do with it:

```
yarn add styled-components 
OR
npm install --save styled-components
```

## Our first styling

The example the homepage for this library uses is that of buttons. You might end up creating different buttons meant for different purposes in your app. You might have default buttons, primary buttons, disabled buttons and so on. Styled components lib enable you to keep all that CSS in one place. Let's start off by importing it:

```
import styled from 'styled-components';
```

Now to use it we need to use backticks \`, like so:

    const Button = styled.button``;

As we can see above we call styled.nameOfElement\`\` to define a style for our element. Let's add some style to it:

    const Button = styled.button`
      background: black;
      color: white;
      border-radius: 7px;
      padding: 20px;
      margin: 10px;
      font-size: 16px;
      :disabled {
        opacity: 0.4;
      }
      :hover {
        box-shadow: 0 0 10px yellow;
      }
    `;

What we can see from the above example is that we are able to use normal CSS properties in combination with pseudo selectors like `:disabled` and `:hover` .

## Using attributes

We can look at the attributes of our element and determine the styling. Imagine that we wanted to create something else, like a primary button. Then we might in the markup declare it in the following way:

```
<Button primary >I am a primary button</Button>
```

Styled components can easily pick up on this attribute by doing the following:

    const Button = styled.button`
      background: black;
      color: white;
      border-radius: 7px;
      padding: 20px;
      margin: 10px;
      font-size: 16px;
      :disabled {
        opacity: 0.4;
      }
      :hover {
        box-shadow: 0 0 10px yellow;
      }

       ${props => props.primary && css`
        background: green;
        color: white;
      `}
    `;

Let's zoom in on the needed addition:

    ${props => props.primary && css`
      background: green;
      color: white;
    `}

by using `${props}` we are able to write code that checks the attributes of the element, `props` wether they contain the property `primary`. Thereafter we use `&& css` to specify what additional CSS we should be adding if the condition is met. 

