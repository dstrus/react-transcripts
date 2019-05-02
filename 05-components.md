Hey, everyone. Welcome to Kenzie Academy. I'm Davey Strus.

We've established that React is a JavaScript library for building user interfaces, and that we build those interfaces with reusable UI components. What was it the documentation said about components?

> Components let you split the UI into independent, reusable pieces, and think about each piece in isolation.

That sounds really handy. We should learn how to make components.

```js
(<header>
  <h1>Welcome to my site</h1>
  <ul>
    <li><a href="home">Home</a></li>
    <li><a href="about">About</a></li>
    <li><a href="archives">Archives</a></li>
    <li><a href="contact">Contact</a></li>
  </ul>
</header>)
```

Remember this JSX expression? That's a lot better than the non-JSX version, but what if we could replace all of that...

```js
<AppHeader />
```

...with this? We can, by turning it into a component. Let's look at a component now.

```js
function Welcome(props) {
  return <h1>Welcome, Davey!</h1>;
}
```

Here we are: A component called `Welcome`. It's just a function that returns a JSX expression. That's all the simplest components are: Functions that return some JSX. Really, it returns a React element, but we know that the easiest way to create React elements is to use JSX. Notice that the function takes an argument called `props`.

If we think of functions as factories, a component is a factory that takes inputs called `props` and returns a React element that describes what visitors should see in the browser.

When we create components as functions, they must be _pure_ functions: **Pure functions** are functions that don't modify their inputs, and that always return the same output given the same inputs. But components aren't always functions. They can also be classes.

```js
class Welcome extends React.Component {
  render() {
    return <h1>Welcome, Davey!</h1>;
  }
}
```

Here's the same component as a class. What's different?

Well, we have a class named `Welcome` instead of a function named `Welcome`.

The class inherits from `React.Component`. More on that in a minute.

And we implemented a render method, which has the same body as the functional component. All right. Not too bad. But what's the point? Why bother with a class component?

Maybe you just really like ES6 class syntax. Once your components get a little more complex, you may find that you need some additional functions, and classes with methods are a nice way to do that. Strictly speaking, you _can_ make class components without using ES6 syntax, but only do that if you have a really specific use case. Why else would you use a class component?

Class components can maintain internal **state**, whereas functional components are **stateless**. State gives a component its own private data that it has full control over, and sometimes that's exactly what you need. We'll learn more about state in a later video.

With class components, we also have access to **lifecycle hooks**. If we implement certain methods, they will automatically execute at specific times in the lifecycle of that component.

```js
class Welcome extends React.Component {
  render() {
    return <h1>Welcome, Davey!</h1>;
  }
}
```

Inheriting from `React.Component` gives us what we need to work with state and lifecycle hooks. That's why we can't use those things in functional components.

Now that we know what components are and how to make them, let's actually do it!

Let's add a component at the beginning of our script here.

```html
  <script type="text/babel">
    function Welcome(props) {
      return <h1>Welcome, Davey!</h1>;
    }

    const element = <h1>Hello, JSX!</h1>;
    ReactDOM.render(
      element,
      document.getElementById('root')
    );
  </script>
```

We'll write a function called `Welcome` that takes a single argument called `props`. All this function does is return some JSX.

```html
  <script type="text/babel">
    function Welcome(props) {
      return <h1>Welcome, Davey!</h1>;
    }

    const element = <Welcome />;
    ReactDOM.render(
      element,
      document.getElementById('root')
    );
  </script>
```

So how do we use this component, now that we have it? You can just use it in JSX like any other element. Check this out! The element doesn't need any contents, so we'll use a self-closing tag. Does it work? Let's try it out.

Yep, it sure does!

```html
  <script type="text/babel">
    function Welcome(props) {
      return <h1>Welcome, Davey!</h1>;
    }

    ReactDOM.render(
      <Welcome />,
      document.getElementById('root')
    );
  </script>
```

You don't need to set that JSX expression to a variable before you use it in `ReactDOM.render()`. We could just pass that self-closing `<Welcome />` tag to `render()`. It still works.

Now you know how to make a component, and how to use it on a page. But components have props and state and stuff like that. We'll have to learn more about those. We'll dig into props next time. Until then, I'm Davey Strus. Haaaappy hacking!
