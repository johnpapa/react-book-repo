# Thinking in components

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



