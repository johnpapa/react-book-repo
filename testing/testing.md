# Jest
Jest sells itself by saying its 
> Delightful JavaScript testing


What makes is delightful? It boosts that it has a *zero-configuration* experience. Ok, we are getting closer to the answer.

- Great performance by tests running in parallell thanks to workers.
- Built in coverage tool
- Works with typescript thanks to [ts-jest](https://github.com/kulshekhar/ts-jest)
 
## Get started 
Let's try to set it up and see how much configuration is needed.
If you just want to try it there is a [Jest REPL](https://repl.it/languages/jest) where you will be able to write tests among other things.

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
We see above that we are using `describe` to create a test suite and `it` to create a test within the test suite. We also see that we use `expect` to assert on the result. The `expect` give us access to a lot of matchers, a matcher is the function we call after the `expect`:

```
expect(something).matcher(value)
```
As you can see in our test example we are using a matcher called `toBe()` which essentially matches what's inside the `expect` to whats inside the matcher, example:

```
expect(1).toBe(1) // succeeds
expect(2).toBe(1) // fails
```

There are a ton of matchers so I urge you to have a look at the ones that exists and try to use the appropriate matcher [Matchers](https://facebook.github.io/jest/docs/en/expect.html)

### Running our test
The simplest thing we can do is just to create a project using `create-react-app`, cause the Jest is already set up in there. Once we have the project created and all dependencies installed we can simply run:

```
yarn test
```
![](/assets/Screen Shot 2018-05-31 at 14.30.30.png)
It will show the above image. One executed test suite, one passing tests and host of commands that we will explore in a bit. It seems to have run the file `src/App.test.js`. Let's have a look at said file:

```js
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
 
How bout we try adding those add.js file and its corresponding test?

Let's first add add.js, like so:

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
Let's have a look at our terminal again and especially the bottom of it:
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
Now this one is a bit IDE dependant, for this section we will cover how to do this VS Code. 
First thing we are going to do is install an extension. Head over to the extension menu and search for `Jest`. The following should be showing:
![](/assets/Screen Shot 2018-06-01 at 15.19.07.png)
Install this extension and head back to your code. Now we have some added capabilities. All of our tests should have a `Debug` link over every single test. At this point we can add a breakpoint and then press our `Debug` link. Your break point should now be hit and it should look like so:
![](/assets/Screen Shot 2018-06-01 at 15.21.44.png)
TODO
#### Run in a browser
TODO
## Snapshot testing
Snapshot is about creating a snapshot, a view of what the DOM looks like when you render your component. It's used to ensure that when you or someone else does a change to the component the snapshot is there to tell you, you did a change, does the change look ok?

If you agree with the change you made you can easily update the snapshot with what DOM it now renders. So snapshot is your friend to safeguard you from unintentional changes.

Let's see how we can create a a snapshot:

