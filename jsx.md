# JSX

JSX is pretty much you writing XML in JavaScript. It's a preprocessor step. You don't have to have it but it makes life a whole lot easier.

## Simple example

This is a simple example on one line of code:

```js
const Elem = <h1>Some title</h1>;

// and you can use it in React like so:
<div>
  <Elem />
</div>
```

The above looks like XML in JavaScript. When it is being processed it is turned into the following ES5 code:

```js
React.createElement('Elem', null, 'Some title');
```

Ok so Elem becomes the element name, the second argument that above is null are our element attributes, which we don't have any. The third and last argument is the elements value. Let's look at an example below where we give it a property:

```js
const Elem = <h1>Some title</h1>;

// usage:
<div>
  <Elem title="a title">
</div>
```

The above would become the following code:

```js
React.createElement('Elem', { title: 'a title' }, 'Some title');
```



