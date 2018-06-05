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
  })

});


```

