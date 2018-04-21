# Methods

When you build your component you are going to want to add methods to it. You are going to attach methods to different events such as `submit`, `click`, `change` etc. One thing you need to keep in mind is that React changes the name and the casing of the event like so:

* click becomes onClick
* change becomes onChange
* submit becomes onSubmit 

## Event examples

Let's have a look at how to set up a method to an event:

```
class Element extends React.Component {
 clicked() {
   console.log('clicked'); 
 }
 
 render() {
   return (
     <button onClick={this.clicked}></button>
   )
 }
}
```

## Binding our method to our class

This looks all well and good but it has a problem. You don't see the problem right now cause it does what it is supposed to i.e print clicked in the console. However try do the following modification:

```
class Element extends React.Component {
 state = {
   str: 'test'
 } 

 clicked() {
   console.log('clicked ' + this.state.str); 
 }
 
 render() {
   return (
     <button onClick={this.clicked}></button>
   )
 }
}
```

The above code WILL give out an error as it doesn't know what state is. This is because our **this** points wrong. There are several ways to fix this. Let's look at the first one:

```
class Element extends React.Component {

  constructor() {
    super();
    this.clicked = this.clicked.bind(this);
  }

 state = {
   str: 'test'
 } 

 clicked() {
   console.log('clicked ' + this.state.str); 
 }
 
 render() {
   return (
     <button onClick={this.clicked}></button>
   )
 }
}
```

We are above declaring a constructor and binding our method `clicked()` to the object instance, like so:

```
constructor() {
    super();
    this.clicked = this.clicked.bind(this);
  }


```

### Two other versions of binding the method

At this point our code works again but it doesn't feel all too pretty. Is there a better way? Actually there are two more ways we could solve this:

* declare our method as a const parameter in the class
* invoke our method as a lambda

Let's look at the first mentioned variant:

In this 





