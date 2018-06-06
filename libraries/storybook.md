# Storybook
Imagine when your project grows and the number of components and developers with it. It might get hard to keep track of all the components and how they are rendered differently depending on what input they get. For those situations there exists a number of tools where you can visually display your components - a style guide. This article is about one such tool called Storybook. 

## Install & Setup
How would we install it? We need to install a CLI for this by typing:

```
npm i -g @storybook/cli
```

Once we have done so we can create our React project and from the project root we can type:

```
getstorybook
```
This will install all the needed dependencies plus:

- add a `storybook-demo/.storybook/` directory
- `src/stories`
- add the following to `package.json` and the `scripts` tag: "storybook": "start-storybook -p 9009 -s public"

The `.storybook` directory contains the following:
- addons.js
- config.js
`config.js` tells `storybook` where our *stories* can be found.
The `stories` directory contains an `index.js` and *stories* which is something that storybook picks up and renders as a HTML page. 

The `/stories/index.js` looks like this:
```js
import React from 'react';

import { storiesOf } from '@storybook/react';
import { action } from '@storybook/addon-actions';
import { linkTo } from '@storybook/addon-links';

import { Button, Welcome } from '@storybook/react/demo';

storiesOf('Welcome', module).add('to Storybook', () => <Welcome showApp={linkTo('Button')} />);

storiesOf('Button', module)
  .add('with text', () => <Button onClick={action('clicked')}>Hello Button</Button>)
  .add('with some emoji', () => (
    <Button onClick={action('clicked')}>
      <span role="img" aria-label="so cool">
        😀 😎 👍 💯
      </span>
    </Button>
  ));
```

## Run
By running
```
yarn storybook 
```
It will startup story book and we can navigate to `http://localhost:9000`. It will look like this:

![](/assets/Screen Shot 2018-06-06 at 14.23.34.png)

Let's look at the code that produces this in `stories/index.js`. Every call `storiesOf('name of module', module)` produces a section. We can then chain a call to id `add('name of component variant', () => ())`. Now the `add` method is worth looking closer at:

```js
.add('with text', () => <Button onClick={action('clicked')}>Hello Button</Button>)
``` 
The second argument renders out a React component and we seem to be able to set whatever property we want in there. Let's try to create a component and add it to `stories/index.js` next.
## Add a story
Let's do the following:

- create a `Todos.js` component
- add an entry of that component to `stories/index.js`

 First off the code for `Todos.js`:
 
```
// Todos.js
import React from 'react';
import styled from 'styled-components';

const Todo = styled.div`
  box-shadow: 0 0 5px grey;
  padding: 10px;
  margin-bottom: 10px;
`;

const Todos = ({ todos }) => (
  <React.Fragment>
    <h3>List of todos</h3>
    {todos.map(t => <Todo key={t.title}>{t.title}</Todo>)}
  </React.Fragment>
);

export default Todos;
``` 
Now let's add the entry to `stories/index.js`:

```js
// stories/index.js

import React from 'react';

import { storiesOf } from '@storybook/react';
import { action } from '@storybook/addon-actions';
import { linkTo } from '@storybook/addon-links';

import { Button, Welcome } from '@storybook/react/demo';
import Todos from '../Todos';

import mocks from './mocks';

storiesOf('Welcome', module).add('to Storybook', () => <Welcome showApp={linkTo('Button')} />);

storiesOf('Button', module)
  .add('with text', () => <Button onClick={action('clicked')}>Hello Button</Button>)
  .add('with some emoji', () => (
    <Button onClick={action('clicked')}>
      <span role="img" aria-label="so cool">
        😀 😎 👍 💯
      </span>
    </Button>
  ));

storiesOf('Todos', module)
  .add('with todos', () => <Todos todos={mocks.todos} />)
```

Let's highlight on our additions. First we import what we need:

 ```js
import Todos from '../Todos';
import mocks from './mocks';
 ``` 
 
 The `mocks.js` is a file we created to supply our components with data. We chose to place it in a separate file so we don't clutter up `index.js`. The `mocks.js` is a very simple file looking like this:
 
```js
// stories/mocks.js
const mocks = {
  todos: [{ title: 'test' }, { title: 'test2' }]
};

export default mocks;
```

Now back to our `index.js` file and let's look at the part where we set up our story:

```js
storiesOf('Todos', module)
  .add('with todos', () => <Todos todos={mocks.todos} />)

```
As you can see we set up the story and define a variant of that story by calling `add`. Let's have a look at the result:

![](/assets/Screen Shot 2018-06-06 at 14.46.52.png)

## Improving - Dedicated story folders
