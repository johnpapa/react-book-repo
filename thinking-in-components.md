# Thinking in components - building a todo app

Thinking in components is about being able to break down an application in components. Consider something as simple as a Todo application. It will most likely consist of a number of components. Let's try and break that down.

## A list of todos

We start off by a list of todos and we realise we need to be able to render said list. Before we consider how we should render it out on the screen, let's think about the underlying data structure.

A todo has a description. Next interesting thing to think about is how do we complete a todo. The intention of having a list of things we need to carry out it as at some point in time we will hopefully carry out those items on the list. Now we have to ask ourselves wether we want to remove a finished item in our todo list or simply mark it as completed. We opt for the latter to feel good about ourselves for completing it but also so what we can see what we have carried out historically. This tells us something about our data structure. It should most likely look something like this:

```
[{
 title: 'clean',
 done: false
}, {
 title: 'do dishes',
 done: false
}]
```

The above list seems like a reasonable design of the data structure and now we can turn to trying to render it in React.

## Rendering the list

Most likely we will try out with something like this:

```
{todos.map(todo => (
  <div>
  <input type="checkbox" checked={todo.done} /> {todo.title} 
  </div>
)}
```

At this point we are able to render our todo list but we are not able to change the value of the todo. Let's build this out to a real React component class and add support for changing a todo to done.

    import React, { Component } from 'react';
    import styled from 'styled-components';

    import './App.css';

    const todos = [{
      title: 'clean',
      done: false,
    },
    {
      title: 'do the dishes',
      done: true,
    }];

    const Todos = styled.div`
      padding: 30px;
    `;

    const Todo = styled.div`
      box-shadow: 0 0 5px gray;
      padding: 30px;
      margin-bottom: 10px;
    `;

    class App extends Component {
      render() {
        return (
          <Todos>
            <h2>Todos</h2>
            {todos.map(todo => (
              <Todo>
                <input type="checkbox" checked={todo.done} /> {todo.title}
              </Todo>
              ))}
          </Todos>
        );
      }
    }

    export default App;

Above we have created a fully working component but we are yet to add support for changing our todos. Let's do that next. We need to do the following:

* listen to an onChange event when we check our checkbox
* change the item in our todo list to reflect the change we just made

Listen to onChange is quite simple, we just need to add a method that listens to it like so:

```
<input type="checkbox" checked={todo.done} onChange={() => this.handleChange(todo)} />
```

After that we need to figure out a way to change a todo in our list. We could be altering the todos list directly but the more React thing todo would be to create a todos state in the component, like so:

```
state = {
 todos
}
```

Let's now add the final code:

    import React, { Component } from 'react';
    import styled from 'styled-components';

    import './App.css';

    const todos = [{
      title: 'clean',
      done: false,
      id: 1,
    },
    {
      title: 'do the dishes',
      done: true,
      id: 2,
    }];

    const Todos = styled.div`
      padding: 30px;
    `;

    const Todo = styled.div`
      box-shadow: 0 0 5px gray;
      padding: 30px;
      margin-bottom: 10px;
    `;

    class App extends Component {
      state = {
        todos,
      };

      handleChecked = (todo) => {
        const newTodos = this.state.todos.map(t => {
          if (t.id === todo.id) {
            return { ...t, done: !t.done };
          }
          return t;
        });

        this.setState({
          todos: newTodos,
        });
      }

      render() {
        return (
          <Todos>
            <h2>Todos</h2>
            {this.state.todos.map(todo => (
              <Todo key={todo.id}>
                <input type="checkbox" onChange={() => this.handleChecked(todo)} checked={todo.done} /> 
                {todo.title}
              </Todo>
              ))}
          </Todos>
        );
      }
    }

    export default App;


Let's zoom in on the handleChecked method here to realise what we have done:

```
handleChecked = (todo) => {
  const newTodos = this.state.todos.map(t => {
    if (t.id === todo.id) {
      return { ...t, done: !t.done };
    }
    return t;
  });

  this.setState({
    todos: newTodos,
  });
}
```

we go through the list, todo by todo until we find the selected todo then we change it state by doing an object spread:

```
return { ...t, done: !t.done }
```

Another thing worth noting is that our todo now consist of three properties:

* title
* done
* id

We needed the id property to identify which todo we were trying to modify.

## Create a todos component

So far we have everything inside of the App component and we don't want our entire application to live in there. What if we want to add other stuff. First thing we are going to do is to create a Todos component and thereby move the rendering, state and methods into that component. We will end up with the following files:

* App.js - we had this from the beginning
* Todos.js - this is new

Our Todos.js will contain pretty much all of what App.js used to contain:

    // Todos.js

    import React, { Component } from 'react';
    import styled from 'styled-components';
    import PropTypes from 'prop-types';

    const TodosContainer = styled.div`
      padding: 30px;
    `;

    const Todo = styled.div`
      box-shadow: 0 0 5px gray;
      padding: 30px;
      margin-bottom: 10px;
    `;

    class Todos extends Component {
      static propTypes = {
        todos: PropTypes.array.isRequired,
      }

      state = {
        todos: this.props.todos,
      };

      handleChecked = (todo) => {
        const newTodos = this.state.todos.map(t => {
          if (t.id === todo.id) {
            return { ...t, done: !t.done };
          }
          return t;
        });

        this.setState({
          todos: newTodos,
        });
      }

      render() {
        return (
          <TodosContainer>
            <h2>Todos</h2>
            {this.state.todos.map(todo => (
              <Todo key={todo.id}>
                <input type="checkbox" onChange={() => this.handleChecked(todo)} checked={todo.done} /> {todo.title}
              </Todo>
              ))}
          </TodosContainer>
        );
      }
    }

    export default Todos;


The App.js will now look like the following:

```
import React, { Component } from 'react';

import Todos from './Todos';

import './App.css';

const todos = [{
  title: 'clean',
  done: false,
  id: 1,
},
{
  title: 'do the dishes',
  done: true,
  id: 2,
}];

class App extends Component {
  render() {
    return (
      <Todos todos={todos} />
    );
  }
}

export default App;

```



