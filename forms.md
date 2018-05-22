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
The form itself maintains the state of all its input fields but you tend to want to control that and make React the single source of truth