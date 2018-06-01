# Jest
Jest sells itself by saying it's 
> Delightful JavaScript testing


What makes is delightful? It boosts that it has a *zero-configuration* experience. Ok, we are getting closer to the answer.

- Great performance by tests running in parallell thanks to workers.
- Built in coverage tool
- Works with typescript thanks to [ts-jest](https://github.com/kulshekhar/ts-jest)
 
## Get started 
Let's try to set it up and see how much configuration is needed.
If you just want to try it, there is a [Jest REPL](https://repl.it/languages/jest) where you will be able to write tests among other things.

### Writing our first test
To make the test runner find our tests we need to follow one of three conventions:

- Create a \_\_tests\__\ directory and place your files in there
- Make file match *spec.js
- Make file match .test.js

Ok, so now we know how Jest will find our files, how about writing a test? 

```js
// add.js
function add(a, b) {
  return a + b;
}

module.exports = add;

// __tests__/add.js

const add = require('../add');
describe('add', () => {
  it('should add two numbers', () => {
    expect(add(1, 2)).toBe(3);
  });
});
```
We see above that we are using `describe` to create a test suite and `it` to create a test within the test suite. We also see that we use `expect` to assert on the result. The `expect` gives us access to a lot of matchers, a matcher is the function we call after the `expect`:

```
expect(something).matcher(value)
```
As you can see in our test example we are using a matcher called `toBe()` which essentially matches what's inside the `expect` to what's inside the matcher, example:

```
expect(1).toBe(1) // succeeds
expect(2).toBe(1) // fails
```

There are a ton of matchers so I urge you to have a look at the ones that exists and try to use the appropriate matcher [Matchers](https://facebook.github.io/jest/docs/en/expect.html)

### Running our test
The simplest thing we can do is just to create a project using `create-react-app`, cause Jest is already set up in there. Once we have the project created and all dependencies installed we can simply run:

```
yarn test
```
![](/assets/Screen Shot 2018-05-31 at 14.30.30.png)

It will show the above image containing: 
- One executed test suite, 
- one passing tests 
and host of commands that we will explore in a bit. It seems to have run the file `src/App.test.js`. Let's have a look at said file:

```js
// src/App.test.js

import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';

it('renders without crashing', () => {
  const div = document.createElement('div');
  ReactDOM.render(<App />, div);
  ReactDOM.unmountComponentAtNode(div);
});
```
As we can see it has created a test using `it` and have also created a component using `ReactDOM.render(<App />, div)`, followed by cleaning up after itself by calling `ReactDOM.unmount(div)`. We haven't really done any assertions at this point, we have just tried to create a component with no errors as the result, which is good to know though.
 
How bout we try adding `add.js` file and its corresponding test?

Let's first add `add.js`, like so:

```js
// add.js

function add(a,b) {
  return a + b;
}

export default add;
```

followed by the test

```js
// __tests__/add.js

import add from '../add';

it('testing add', () => {
  const actual = add(1,3);
  expect(actual).toBe(4);
});
```
Our Jest session is still running in the terminal:
![](/assets/Screen Shot 2018-05-31 at 14.40.56.png)

We can see that we now have two passing tests.

### Debugging
Any decent test runner/framework should give us the ability to debug our tests. It should give us the ability to:
- run specific tests
- ignore tests
- let us add breakpoints in our IDE (more up to the IDE vendor to make that happen)
- let us run our tests in a Browser

#### Run specific test files
Let us look at how to do these things, let's start with running specific tests. First off we will add another file `subtract.js` and a corresponding test.

```js
// subtract.js

function subtract(a,b) {
  return a - b;
}

export default subtract;
```
and the test:

```js
// __tests__/subtract.js
import subtract from '../subtract';

it('testing subtract', () => {
  const actual = subtract(3,2);
  expect(actual).toBe(1);
});
```
Let's have a look at our terminal again and especially at the bottom of it:
![](/assets/Screen Shot 2018-05-31 at 14.48.47.png)

If you don't see this press `w` as indicated on the screen. The above give us a range of commands which will make our debugging easier:

- `a`, runs all the tests
- `p` this will allow us to specify a pattern, typically we want to specify the name of a file here to make it only run that file.
- `t` it does the same as `p` but it let's us specify a test name instead
- `q`, quits the watch mode
- `Enter`, to trigger a test run

Given the above description we will try to filter it down to only test the add.js file so we type `p`:
![](/assets/Screen Shot 2018-05-31 at 14.52.06.png)

This takes us to a pattern dialog where we can type in the file name. Which we do:
![](/assets/Screen Shot 2018-05-31 at 14.52.18.png)

Above we can now see that only the add.js file will be targeted.

#### Run specific tests
We have learned how to narrow it down to specific files. We can narrow it down to specific tests even using the `p`, pattern approach. First off we will need to add a test so we can actually filter it down:

```js
// __tests__/add.js

import add from '../add';

it('testing add', () => {
  const actual = add(1,3);
  expect(actual).toBe(4);
});

it('testing add - should be negative', () => {
  const actual = add(-2,1);
  expect(actual).toBe(-1);
});
```
At this point our terminal looks like this:
![](/assets/Screen Shot 2018-05-31 at 14.57.19.png)

So we have two passing tests in the same file but we only want to run a specific test. We do that by adding the `.only` call to the test, like so:

```js
it.only('testing add', () => {
  const actual = add(1,3);
  expect(actual).toBe(4);
});
```
and the terminal now looks like so:
![](/assets/Screen Shot 2018-05-31 at 14.58.49.png)

We can see that adding `.only` works really fine if we only want to run that test. We can use `.skip` to make the test runner skip a specific test:

```js
it.skip('testing add', () => {
  const actual = add(1,3);
  expect(actual).toBe(4);
});
```
The resulting terminal clearly indicated that we skipped a test:
![](/assets/Screen Shot 2018-05-31 at 15.00.42.png)

#### Debugging with Break points
Now this one is a bit IDE dependant, for this section we will cover how to do this in VS Code. 
First thing we are going to do is install an extension. Head over to the extension menu and search for `Jest`. The following should be showing:
![](/assets/Screen Shot 2018-06-01 at 15.19.07.png)

Install this extension and head back to your code. Now we have some added capabilities. All of our tests should have a `Debug` link over every single test. At this point we can add a breakpoint and then press our `Debug` link. Your break point should now be hit and it should look like so:
![](/assets/Screen Shot 2018-06-01 at 15.21.44.png)

## Snapshot testing
Snapshot is about creating a snapshot, a view of what the DOM looks like when you render your component. It's used to ensure that when you or someone else does a change to the component the snapshot is there to tell you, you did a change, does the change look ok?

If you agree with the change you made you can easily update the snapshot with what DOM it now renders. So snapshot is your friend to safeguard you from unintentional changes.

Let's see how we can create a a snapshot. First off we might need to install a dependency:

```
yarn add react-test-renderer --save
```
Next step is to write a component and a test to go along with it. It should look something like this:

```js
// Our component
import React from 'react';

const Todos = ({ todos }) => (
  <React.Fragment>
    {todos.map(todo => <div>{todo}</div>)}
  </React.Fragment>
);

export default Todos;
```

```js
// The test

import renderer from 'react-test-renderer';
import React from 'react';

import Todos from '../Todos';

test('Todo - should create snapshot', () => {
  const component = renderer.create(
    <Todos todos={['item1', 'item2']} />,
  );
  let tree = component.toJSON();
  expect(tree).toMatchSnapshot();
})
```
Note how `import`, imports the component we are about to test:

```
import Todos from '../Todos';

```
This is followed by using the `renderer` to create an instance of our component. Next step is turn that component into a JSON representation like so `component.toJSON()` and lastly we assert on this by calling `expect(tree).toMatchSnapshot()`, this will call a snapshot that will place itself in a `__snapshots__` directory under your `tests` directory.

### Managing the snapshot
Ok, so we have a snapshot, now what? Let's do a change to our todo component, like so:

```js
import React from 'react';

const Todos = ({ todos }) => (
  <React.Fragment>
    {todos.map(todo => (
      <React.Fragment>
        <h3>{todo.title}</h3>
        <div>{todo.description}</div>
      </React.Fragment>
    ))}
  </React.Fragment>
);

export default Todos;
```
We see that our `todo` is an object instead of a string so it has a `title` and `description` property. This WILL make our snapshot react and it will say the following:
![](/assets/Screen Shot 2018-06-01 at 15.47.36.png)

It clearly indicates something is different and asks us to inspect the code. If we are happy with the changes we should press `u` to update the snapshot to its new version. So look at the code and `yes` this is an intended change so therefore we press `u`. We end up with the following image telling us everything is ok:

![](/assets/Screen Shot 2018-06-01 at 15.50.38.png)

## Mocking
Mocking is one of those things that needs to work well. Mocking in Jest is quite easy. You need to create your mocks in a directory that is adjacent to your module, or more like a child directory to the module. Let's show what I mean in code. Imagine you have the following module:

```js
// repository.js

const data = [{
  title: 'data from database'
}];

export default data;
```

Let's look at a test for this one:

```js
// __tests__/repository.js

import data from '../repository';

describe('testing repository data', () => {
  it('should return 1 item', () => {
    console.log(data);
    expect(data.length).toBe(1);
  });
});
```
Not the best of tests but it is "a test". Let's create our mock so that our file structure look like so:

```
repository.js
__mocks__/repository.js
```
Our mock should look like this:

```js
// __mocks__/repository.js
const data = [{
  title: 'mocked data'
}];

export default data;
```

To use this mock we need to call `jest.mock()` inside of our test, like so:

```
// __tests__/repository.js

import data from '../repository';
jest.mock('../repository');

describe('testing repository data', () => {
  it('should return 1 item', () => {
    console.log(data);
    expect(data.length).toBe(1);
  });
});
```
Now it uses our mock instead of the actual module. Ok you say, why would I want to mock the very thing I want to test. Short answer is - you wouldn't. So therefore we are going to create another file `consumer.js` that use our `repository.js`. So let's look at the code for that and its corresponding test:

```js
// consumer.js

import data from './repository';
const item = { title: 'consumer' };

export default [ ...data, { ...item}];
```
Above we clearly see how our consumer use our `repository.js` and now we want to mock it so we can focus on testing the consumer module. Let's have a look at the test:

```js
import data from '../consumer';
jest.mock('../repository');

describe('testing consumer data', () => {
  it('should return 2 items', () => {
    console.log(data);
    expect(data.length).toBe(2);
  });
});
```
We use `jest.mock` and mocks away the only external dependency this module had.

What about libs like `lodash` or `jquery`, things that are not modules that we created but is dependant on? We can create mocks for those at the highest level by creating a `__mocks__` directory.

## Coverage
We have come to the final section in this chapter. This is about realizing how much of our code is covered by tests. To check this we just run:

```
yarn test --coverage
```

This will give us a table inside of the terminal that will tell us about the coverage in percentage per file. It will also produce a `coverage` directory that we can navigate into and find a `HTML` report of our coverage. But first off let's change the `add.js` file to add a piece of logic that needs a test, like so:

```js
function add(a,b) {
  if(a > 0 && b > 0 ) {
    return a + b;
  }
  throw new Error('parameters must be larger than zero');
}

export default add;
```
Now we can see we have more than one path through the application. If our input params are larger than zero then we have existing tests that cover it. However if one or more parameters is below zero then we enter a new execution path and that one is NOT covered by tests. Let's see what that looks like in the coverage report by navigating to `coverage/lcov-report`. We can show this by typing for example `http-server -p 5000` and we will get a report looking like this:
![](/assets/Screen Shot 2018-06-01 at 17.26.16.png)

Now we can navigate to `src/add.js` and it should look like this:

![](/assets/Screen Shot 2018-06-01 at 17.27.13.png)

Now we can clearly see how our added code is indicated in red and that we need to add a test to cover that new execution path.

Next we add a test to cover for this, like so:

```
import add from '../add';

describe('add', () => {
  it('testing addition', () => {
    const actual = add(1,2);
    expect(actual).toBe(3);
  })

  it('testing addition with neg number', () => {
    expect(() => {
      add(-1,2);
    }).toThrow('parameters must be larger than zero');
  })
})
```
Our second case should now cover for the execution path that leads to an exception being thrown. Let's rerun our coverage report:

![](/assets/Screen Shot 2018-06-01 at 17.38.01.png)
