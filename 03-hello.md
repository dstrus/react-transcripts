Hey, everyone. Welcome to Kenzie Academy. I'm Davey Strus.

Let's see React in action. We'll start with just about the simplest possible example of React.

## 5 things needed for a React app

1. An HTML page with at least one empty element
2. The React library
3. A renderer
4. A React element
5. A JavaScript call to render the React element to the empty HTML element from step 1

You need 5 things to get React up and running. First, you need an HTML page with at least one empty element, like an empty DIV. Next, you need the React library itself. Then you need a renderer for React. That'll be ReactDOM if you're running React in a web browser. Then you need one or more React elements, and a call to render that React element to the empty HTML element from step 1. Let's give it a shot!

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <title>Hello, React!</title>
</head>
<body>

</body>
</html>
```

We'll start with some HTML 5 boilerplate. I'll put in the title, "Hello, React!"

```html
<body>

  <!-- element where application will be rendered -->
  <div id="root"></div>

</body>
```

Item 1 on our list is an empty element. We'll put that in the body, and although it's not a requirement, the convention is to make it a `div` with the ID `root`, so that's what we'll do.

```html
<body>

  <!-- element where application will be rendered -->
  <div id="root"></div>

  <script crossorigin src="https://unpkg.com/react@16/umd/react.development.js"></script>

</body>
```

Next, we need React itself. We'll load a development build from a CDN. The difference between this file, `react.development.js` and the production build, `react.min.js`, is that the production build has been minified. That is, it's been processed to make the file absolutely as small as possible. That's great for production, because it will save bandwidth and make things faster, but it does make the source code nearly impossible to read. Just in case we want to dig into React's source code, we'll use the development version. The development version also gives us better error messages.

```html
<body>

  <!-- element where application will be rendered -->
  <div id="root"></div>

  <script crossorigin src="https://unpkg.com/react@16/umd/react.development.js"></script>
  <script crossorigin src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>

</body>
```

Next, we need a renderer. Since we're running React in a web browser, as opposed to using React Native, for example, we want ReactDOM. So we'll load that from the CDN as well, and again we'll use the more readable development version.

```html
<body>

  <!-- element where application will be rendered -->
  <div id="root"></div>

  <script crossorigin src="https://unpkg.com/react@16/umd/react.development.js"></script>
  <script crossorigin src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>

  <script>
    React.createElement('h1', null, 'Hello, React!')
  </script>

</body>
```

Now it's time to actually write a tiny bit of JavaScript, so we'll add a `script` tag. We need a React element, which we can create with `React.createElement()`. This capital-R `React` variable is available because we loaded React from the CDN. It's defined in there.

```js
React.createElement('h1', null, 'Hello, React!')
```

`createElement()` takes three arguments. The first argument is what type of element we're creating—an H1 in this case.

The second argument is an object called `props`. We'll talk about `props` in detail later. We don't need it in this example, so I'll just put `null`.

The third argument is for any _children_ of the element. Here we just have a string, so the only child of this H1 will be a text node. But this argument could be another React element, or multiple elements if we add any additional arguments, and those elements could have children of their own. We're keeping it simple here.

```html
  <script>
    const element = React.createElement('h1', null, 'Hello, React!');
  </script>
```

I'll go ahead and store this React element in a variable called `element`.

```html
  <script>
    const element = React.createElement('h1', null, 'Hello, React!');
    ReactDOM.render(
      element,
      document.getElementById('root')
    );
  </script>
```

Finally I'll render the React element to the empty HTML element from step 1. I'll do that with a call to `ReactDOM.render()`. `ReactDOM` is defined because we loaded it from the CDN. It takes two arguments: The React element—our `element` variable in this case—and the DOM element where this React element should be rendered. That's our empty element from step 1. I'll grab it with `getElementById()`. If you didn't give your empty DIV the ID `root`, then you may need to grab the element some other way, maybe with `querySelector()`.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <title>Hello, React!</title>
</head>
<body>
  <div id="root"></div>

  <script crossorigin src="https://unpkg.com/react@16/umd/react.development.js"></script>
  <script crossorigin src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
  <script>
    const element = React.createElement('h1', null, 'Hello, React!');
    ReactDOM.render(
      element,
      document.getElementById('root')
    );
  </script>
</body>
</html>
```

And that's it's. That's the whole thing.

I'll go ahead and load it up in a web browser. I don't need to run a server or anything. I can just open up the HTML file directly. There's my H1. It worked! When I inspect, I see that the H1 is rendered inside that `root` div. Nifty!

So that's the simplest possible React application. If you weren't coding along with me, you should definitely try it out in an HTML file on your machine.

Want to know a secret? Almost no one actually calls `React.createElement()` like this when they use React. There's an extension of JavaScript called JSX that gives us a much friendlier syntax for creating React elements, especially when you start nesting elements. We should learn about that next.

Until then, I'm Davey Strus. Haaaappy hacking!
