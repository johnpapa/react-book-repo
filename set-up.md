# Set up

## unpkg/ script tags

This version is simplest to start with if you are beginner. This enables you to dive straight into React and learn its features.

```js
// app.js

class Application extends React.Component {
  render() {
    return (
      <div>
        App
      </div>
    );
  }
}

ReactDOM.render(<Application />, document.getElementById('app'));
```

```
// index.html

<html>
  <body>
    <!-- This is where our app will live  -->
    <div id="app"></div>

    <!-- These are script tags we need for React, JSX and ES2015 features  -->
    <script src="https://fb.me/react-15.0.0.js"></script>
    <script src="https://fb.me/react-dom-15.0.0.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/babel-core/5.8.34/browser.min.js"></script>
    <script type="text/babel" src="app.js"></script>
  </body>

</html>
```

You can easily serve this up by installing for example http-server

```
npm install http-server -g
```

and type:

```
http-server -p 5000

// should say 'App' in your browser at localhost:5000
```

The drawbacks to the above approach is that everything is compiled at runtime which is horribly slow but its great for learning React, but please don't put it like this in production.

## create-react-app

Create react is a scaffolder developed by Dan Abramov. With it you are able to scaffold a React application with easy and is up and running in no time. The simplest way to get started is downloading it, like so:

```
npm install create-react-app -g
```

Scaffolding an app is almost as simple as downloading it:

```
create-react-app <My app name>
```

create-react-app is a very active project and can truly help you in the beginning of you learning React and even later. Question is really how much you want to be able to configure your project. When you become skilled enough you may come to a point when you don't need create-react-app, but until that point, it is your friend. You can read more on it in the official documentation, [https://github.com/facebook/create-react-app](https://github.com/facebook/create-react-app)

## do it yourself

## 



