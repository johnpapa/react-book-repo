# Nock
Nock is used to mock calls to HTTP. It makes it possible for us to specify what urls to listen to and what to respond with.

## Scenario
Imagine we have the following files:

- products.js, a service that can retrieve data for us
- ProductsList.js, a component that calls a method on products.js to get data and render that

Let's have a look at what these two modules look like:

```js
// products.js

export const getProducts = async () => {
  const response = await fetch('http://myapi.com/products');
  const json = await response.json();

  return json.products;
}
```

Above we can see that we do a `fetch()` call to url `http://myapi.com/products` and thereafter we transform the response snd dig out the data `products`. Let's have a look at the component:

```js
// ProductsList.js

import React from 'react';
import { getProducts } from '../products';

const Products = ({ products }) => (
  <React.Fragment>
    {products.map(p => <div>{product.name}</div>)}
  </React.Fragment>
);

class ProductsContainer extends React.Component {
  state = {
    products: [],
  }

  async componentDidMount() {
    const products = await getProducts();
    this.setState({
      products
    });
  }

  render() {
    return (
      <Products products={this.state.products} />
    );
  }
}

export default ProductsContainer;
```

We can see that we use `product.js` module and call `getProducts()` in the `componentDidMount()` and end up rendering the data when it arrives. 

## Testing it
If we wanted to test ProductsList.js module we would want to focus on mocking away `products.js` cause it is a dependency. We could use the library `nock` for this. Let's start off by installing `nock`, like so:

```js
yarn add nock
``` 

Let's now create a test `__tests__/ProductsList.js` and define it like the following:

```js
// __tests__/ProductsList.js

import React from 'react';
import ReactDOM from 'react-dom';
import ProductsList from '../ProductsList';
import nock from 'nock';

it('renders without crashing', () => {
  const scope = nock('http://myapi.com')
    .get('/products')
    .reply(200, {
      products: [{ id: 1, name: 'test' }]
    }, {
      'Access-Control-Allow-Origin': '*',
      'Content-type': 'application/json'
    });

  const div = document.createElement('div');
  ReactDOM.render(<ProductsList />, div);
  ReactDOM.unmountComponentAtNode(div);
});
```
Let's see first what happens if we don't set up a `nock`:
![](/assets/Screen Shot 2018-06-04 at 13.40.09.png)


Note above how we invoke the `nock()` method by first giving it the baseUrl `http://myapi.com` followed by the path `/products` and the HTTP verb `get` and how we define what the response should look like with `reply()`. We also give the `reply()` method a second argument to ensure `CORS` plays nicely. 



 
