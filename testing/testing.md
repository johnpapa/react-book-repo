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



