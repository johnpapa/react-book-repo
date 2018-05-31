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
To make the rest runner find our tests we need to follow one of three conventions:

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