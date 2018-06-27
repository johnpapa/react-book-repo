# Redux Form

Redux Form is a library that makes it easier to handle forms and make them fit into the Redux system. Let's discuss briefly how we normally approach Forms without using this library. Our approach is usually this:

```js
state = {
  value: ''
}

onSubmit = (evt) => {
  // check the values, if ok, submit
}

onChange = (evt) => {
  this.setState({
    input: evt.target.value
  })
}

render() {
  return (
    <form onSubmit={this.onSubmit}>
      <div>
        <input onChange={this.onChange} value={this.state.value}>
      </div>
    </form>
}
```

Above we show a few things we need to do work with forms, which is

* keep track of when the value of inputs change, using `onChange`
* respond to a submit, using `onSubmit`

There are other things we want to keep track of in a form like:

- validating input, this is a big topic as it could be everything from checking wether an input is there at all or matching some pattern or may even need to check on server side wether something validates or not
- check if its pristine, untouched, if so no need to validate and definitely we shouldn't allow a submit to happen

Here is the good news, Redux Form library helps us with all these things, so let's show the awesomeness of this library in this chapter.




