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

 
