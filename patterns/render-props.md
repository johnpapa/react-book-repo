# Render props

What is Render props? Render props is way for us to create a component that provides some kind of data to another component. Why would we want that? Well imagine that we wanted to o something of the following:

* fetching data
* paging
* accordion

If you have any of the scenarios above you have reusable functionality. Wouldn't it be nice if we can create components for this functionality and just serves it up to some component. That child component would be unaware that it is being served up data. In a sense this resembles what we do with Providers but also how container components wraps presentation components. This all sounds a bit vague so let's show some markup how this could look:

```js
const ProductDetail = ({data}) => (
  <React.Fragment>
    <h2>{data.title}</h2>
    <div>{data.description}</div>
  </React.Fragment>
)

<Fetch url="some url where my data is" render={(data) => 
  <ProductDetail product={data.product} />
}>
```

As we can see above we have two different components `ProductDetail` and `Fetch`. `ProductDetail` just looks like a presentation component. `Fetch` on the other hand looks a bit different. It has a property `url` on it and it seems like it has a `render` property that ends up rendering our `ProductDetail`. We can reverse engineer this and figure out how `Fetch` might look like:

```js

``` 

