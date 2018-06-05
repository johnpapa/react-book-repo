# React testing library

> Simple and complete React DOM testing utilities that encourage good testing practices

It's a lightweight solution for testing React components. It provides utility functions on top of `react-dom` and `react-dom/utils`. Your tests work on DOM nodes over React component instances. 

## Writing a test
Let's look at a real scenario and see what we mean. We will create:

- Todos.js a component that allows you to render a list of Todos and it also allows you to select a specific Todo item
- Todos.test.js, our test file 

Our component code looks like this:

```js
// Todos.js

import React from 'react';
import './Todos.css';

const Todos = ({ todos, select, selected }) => (
  <React.Fragment>
    {todos.map(todo => (
      <React.Fragment key={todo.title}>
        <h3 className={ selected && selected.title === todo.title ? 'selected' :'' }>{todo.title}</h3>
        <div>{todo.description}</div>
        <button onClick={() => select(todo)}>Select</button>
      </React.Fragment>
    ))}
  </React.Fragment>
);

class TodosContainer extends React.Component {
  state = {
    todo: void 0,
  }

  select = (todo) => {
    this.setState({
      todo,
    })
  }

  render() {
    return (
      <Todos { ...this.props } select={this.select} selected={this.state.todo}  />
    );
  }
}

export default TodosContainer;
```

Now to the test:

```js
// Todos.test.js

import {render, Simulate, wait} from 'react-testing-library';
import React from 'react';
import 'jest-dom/extend-expect'
import Todos from '../Todos';

const todos = [{
  title: 'todo1'
},
{
  title: 'todo2'
}];

describe('Todos', () => {
  it('finds title', () => {
    const {getByText, getByTestId, container} = render(<Todos todos={todos} />);
  })

});
```
We can see from the above code that we are using some helpers from `react-testing-library`:
- `render`, this will render our component
- `Simulate`, this will help us trigger things like a `click` event or change the input data for example
- `wait`, this allows us to wait for an element to appear

Looking at the test itself we see that when we call `render` we get an object back that we do destructuring on:

```js
const {getByText, getByTestId, container} = render(<Todos todos={todos} />)
```
and we end up with the following helpers:
- `getByText`, this grabs an element by it's text content
- `getByTestId`, this grabs an element by  `data-testid`, so if you have an attribute on your element like so  `data-testid="saved` you would be querying it like so `getByTestId('saved')`
- `container`, the div your component was rendered to
get

Let's fill in that test:

```js
// Todos.test.js

import {render, Simulate, wait} from 'react-testing-library'
import React from 'react';
import 'jest-dom/extend-expect'
import Todos from '../Todos';

const todos = [{
  title: 'todo1'
},
{
  title: 'todo2'
}];

describe('Todos', () => {
  it('finds title', () => {
    const {getByText, getByTestId, container} = render(<Todos todos={todos} />);
    const elem = container.querySelector('h3');
    expect(elem.innerHTML).toBe('todo1');
  })
});
```

As we can see above we are able to render our element and is also able query for an `h3` element by using the `container` and the `querySelector`. Finally we assert on it. 

### Handling actions

Let's have a look at our component again. Or rather let's look at an excerpt of it:

```js
// excerpt of Todos.js

const Todos = ({ todos, select, selected }) => (
  <React.Fragment>
    {todos.map(todo => (
      <React.Fragment key={todo.title}>
        <h3 className={ selected && selected.title === todo.title ? 'selected' :'' }>{todo.title}</h3>
        <div>{todo.description}</div>
        <button onClick={() => select(todo)}>Select</button>
      </React.Fragment>
    ))}
  </React.Fragment>
);
```
We see above that we try to set the CSS class `selected` if a todo is selected. The way to get a Todo is selected is to click on it, we can see how we invoke the `select` method when we click on the button that is rendered by item. Let's try to test this out by adding a test:

```js
import {render, Simulate, wait} from 'react-testing-library'
import React from 'react';
import 'jest-dom/extend-expect'
import Todos from '../Todos';

const todos = [{
  title: 'todo1'
},
{
  title: 'todo2'
}];

describe('Todos', () => {
  it('finds title', () => {
    const {getByText, getByTestId, container} = render(<Todos todos={todos} />);
    const elem = container.querySelector('h3');
    expect(elem.innerHTML).toBe('todo1');
  })

  it('select todo', () => {
    const {getByText, getByTestId, container} = render(<Todos todos={todos} />);
    Simulate.click(getByText('Select'));
    const elem = container.querySelector('.selected');
    expect(elem).toBeTruthy();
  })
});
```

Our last added test is using the `Simulate` helper to perform a click and we can see that we are using the `getByText` helper to find the button. Thereafter we again use the `container` to the `selected` CSS class. 

## Asynchronous tests and working with input
We have so far shown you how to render a component, find the resulting elements and assert on them. We have also shown how you can carry out things like a click on a button. In this section we will show two things:

- dealing with asynchronous actions
- working with input

We will build the following:

- Note.js a component that allows us enter data and save down the results, it will also allow us to fetch data
- `__tests__/Note.js`, the test file

Let's have a look at the component:

```js
// Note.js
import React from 'react';

class Note extends React.Component {
  state = {
    content: '',
    saved: '',
  };

  onChange = (evt) => {
    this.setState({
      content: evt.target.value,
    });
    console.log('updating content');
  }

  save = () => {
    this.setState({
      saved: `Saved: ${this.state.content}`,
    });
  }

  load = () => {
    var me = this;
    setTimeout(() => {
      me.setState({
        data: [{ title: 'test' }, { title: 'test2' }]
      })
    }, 3000);
  }

  render() {
    return (
      <React.Fragment>
        <input placeholder="change text" onChange={this.onChange} />
        <div data-testid="saved">{this.state.saved}</div>
        {this.state.data &&
        <div data-testid="data">
          {this.state.data.map(item => (
            <div className="item" >{item.title}</div>
          ))}
        </div>
        }
        <div>
          <button onClick={this.save}>Save</button>
          <button onClick={this.load}>Load</button>
        </div>
      </React.Fragment>
    );
  }
}

export default Note;

```


