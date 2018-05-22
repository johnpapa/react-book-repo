#Forms
The default behaviour when submitting a form is form to post the result and move on to another page. Your markup usually looks like so:

```js
<Form>
  <input type="text" name="name" id="name" />
  <button>Submit</button>
</Form>
```

##Controlled components
In React however you tend to want to control Forms a bit more. You usually wants to:

 - verify a form is a valid before being submitted
 - give error message on input field before submitting so you can correct the answer
 
You can verify a forms validity by listening to `onSubmit()` method. That will give you the form as input and you are able to inspect and determine wether the forms data should be persisted. It will look like this:

```js
class App extends Component {
  onSubmit = (ev) => {
    console.log('form', ev.target);

    const { name } = ev.target;
    // value
    console.log('name field', name);
    return false;
  }

  render() {
    return (
      <div className="App">
        <form onSubmit={this.onSubmit}>
          <input name="name" id="name" />
          <button>Save</button>
        </form>
      </div>
    );
  }
}
```
 
###Single source of truth 
The form itself maintains the state of all its input fields but you tend to want to control that and make React the single source of truth. We can do that by putting each element value in the state, like so:

```js
class App extends Component {
  state = {
    firstname: void 0,
  }

  onSubmit = (ev) => {
    console.log('form', ev.target);
    return false;
  }

  handleChange = (ev) => {
    this.setState({
      firstname: ev.target.value,
    });
  }

  render() {
    return (
      <div className="App">
        <form onSubmit={this.onSubmit}>
          <div>
            <label>First name</label>
            <input name="firstname" id="firstname" value={this.state.firstname} onChange={this.handleChange} />
            {this.state.firstname}
          </div>
          <div>
            <button>Save</button>
          </div>
        </form>
      </div>
    );
  }
}
```
Above we are creating the method `handleChange()` that reacts every time the `change` event is triggered. We can see that we subscribe to `change` event when we connect the `handleChange()` method to the `onChange`, like so:

```js
<input name="firstname" 
       id="firstname" 
       value={this.state.firstname} 
       onChange={this.handleChange} 
/>
```

### Adding more input elements
So far we have seen how we can add an input element and hook up a method to `onChange` and stick the value of the element into the state object. Does this means we will have 20 different methods if we have 20 different inputs? No, we can solve this in an elegant way:

```js
class App extends Component {
  state = {
    firstname: void 0,
    lastname: void 0,
  }

  onSubmit = (ev) => {
    console.log('form', ev.target);
    return false;
  }

  handleChange = (ev) => {
    this.setState({
      [ev.target.name]: ev.target.value,
    });
  }

  render() {
    return (
      <div className="App">
        <form onSubmit={this.onSubmit}>
          <div>
            <label>First name</label>
            <input name="firstname" id="firstname" value={this.state.firstname} onChange={this.handleChange} />
            {this.state.firstname}
          </div>
          <div>
            <label>Last name</label>
            <input name="lastname" id="lastname" value={this.state.lastname} onChange={this.handleChange} />
            {this.state.lastname}
          </div>
          <div>
            <button>Save</button>
          </div>
        </form>
      </div>
    );
  }
}
```
