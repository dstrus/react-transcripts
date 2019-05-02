Hey, everyone. Welcome to Kenzie Academy. I'm Davey Strus.

Let's talk about state.

Props are received from outside the component and cannot be changed inside the component. If a component needs to keep track of _and modify_ its own data, we use **state**.

You may remember that functional components are **stateless**. So we'll be dealing exclusively with class components this time.


Like props, state is just an object with any number of properties containing the data we're interested in. Unlike props, state is not passed in via attributes, but is managed internally in the component. Since the UI is usually dependent on state, we generally initialize it in the component's constructor. That way, it will already be there before the initial render.

```js
class Counter extends React.Component {
  constructor(props) {
    super(props);

    this.state = {
      count: 0
    };
  }

  render() {
    return <div>{this.state.count}</div>;
  }
}
```

Here's the start of a `Counter` component. Let's break it down.

It's a stateful component, so we create a class that inherits from `React.Component`.

Skipping ahead to our `render()` method, we return a `div` containing the value of `this.state.count`. That means that our UI depends on that value from state, so we'd better be sure to initialize it. We do that in the constructor.

Functional components just receive `props` as an argument, so perhaps it's no surprise that class components receive props as an argument to their constructor. The constructor we inherit from `React.Component` does something with `props`, so if we're going to implement our own constructor, we need to be sure to call `super()` to run the inherited constructor.

Now then, we'll initialize the state object—`this.state`—with any properties that we plan to use in the component. This setting of the initial value is the only time you'll set state or any of its properties via assignment. If and when you need to update a value in state, you must call the method `this.setState()`. More on that in a minute. Let's actually try this out.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Hello, React!</title>
</head>
<body>

  <!-- element where application will be rendered -->
  <div id="root"></div>

  <script crossorigin src="https://unpkg.com/react@16/umd/react.development.js"></script>
  <script crossorigin src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
  <script src="https://unpkg.com/babel-standalone@6/babel.min.js"></script>

  <script type="text/babel">

  </script>
</body>
</html>
```

I'll start with an HTML 5 page with an empty `#root` DIV. I've included React, ReactDOM, and Babel from their respective CDNs. And I have a script tag with the `type` attribute set to `text/babel`. My code goes inside that `script` tag.

```html
  <script type="text/babel">
    class Counter extends React.Component {
      render() {
        return <div>1</div>;
      }
    }
  </script>
```

Recall the checklist of items that we need in every React app. We have the HTML page with an empty element. We have React and ReactDOM, so next we need a component. Let's start by making it as simple as possible before we add `state`. We need a class, which we'll name `Counter`. It must extend `React.Component`. The class needs a `render()` method, which must return some JSX. I'll just return a DIV with anything in it, just to get a working component. All right. Now we have a component.

```html
  <script type="text/babel">
    class Counter extends React.Component {
      render() {
        return <div>1</div>;
      }
    }

    ReactDOM.render(
      <Counter />,
      document.getElementById('root')
    );
  </script>
```

To get a working app, we need a call to `ReactDOM.render()`, so let's do that. No problem. The first argument is a JSX tag for our `Counter` component. The second argument is that `#root` DIV, which we'll grab with `getElementById()`.

Load it up in a browser, and you should see `1`. Great. Now let's add state.

```js
class Counter extends React.Component {
  constructor(props) {
    super(props);

    this.state = {
      count: 0
    };
  }

  render() {
    return <div>1</div>;
  }
}
```

We need a constructor. Let's add it, making sure to include the `props` argument. We'll pass that along to `super()`, to make sure the constructor from `React.Component` still runs and does whatever setup it's responsible for. Then we initialize state. We have just one property on state: `count`,and we'll initialize it to `0`.

```js
class Counter extends React.Component {
  render() {
    return <div>{this.state.count}</div>;
  }
}
```

Let's render that value from state inside that DIV: `this.state.count`. Now, what do you expect to see when we save and refresh the page?

Zero! Because that's the initial value of `this.state.count`. Very good.

```js
this.setState({
  count: 10
});
```

Now if we ever want state to change, we can't just reassign `this.state.count`. We have to use the method `this.setState()`. You pass in an object containing only the properties that you wish to update. If you leave a property out of that object, it will be left as-is in state. Get it?

```js
this.setState({
  count: this.state.count + 1
});
```

Here's where it gets a little tricky. What you see here can cause problems. If you're basing the new state on a value from the old state, you need to use a callback, because state updates can happen asynchronously, and the value you read as `this.state` may be out-of-date by the time the update is actually applied.

```js
this.setState((state, props) => ({
  count: state.count + 1
}));
```

For these situations, `setState()` takes a callback that receives the previous state as the first argument, and the props at the time the update is applied as the second argument. You can safely use these to calculate the new state.

OK, so that's how you use state. What of it? How is this any better than just using properties on the object itself?

```js
this.count
```

Like `this.count`? How is `this.state.count` any better, and not just more work?

Well, the cool thing about state is that when state changes, the application will re-render. So if part of your UI depends on a value from state, a change to state will automatically redraw the portion of the UI that depends on that value. You don't have to manually update the UI. Cool! So how can we try that out?

```js
class Counter extends React.Component {
  constructor(props) {
    super(props);

    this.state = {
      count: 0
    };
  }

  render() {
    return <div>{this.state.count}</div>;
  }
}
```

Hmm. How can we see both the initial state and an updated state? What about a `setTimeout()`? Kind of contrived, but it will illustrate the point. Fair warning: We won't be keeping any of these changes long-term, so if you want to just watch me instead of typing along during this next bit, that's fine.

```js
      render() {
        setTimeout(() => {
          console.log('boom');
          this.setState({
            count: 22
          });
        }, 1500);

        return <div>{this.state.count}</div>;
      }
    }
```

I'll put this before the `return` statement in the `render()` method. So, `setTimeout()`, put a callback in here. We'll call `this.setState()` and just set it to anything but `0`. How about `22`? All right. We'll wait 1.5 seconds, so `1500`. Let's try it out.

OK, cool. It changes after 1.5 seconds. So is this only happening once? It's not. It's happening roughly every 1.5 seconds, even though we used `setTimeout()`, not `setInterval()`. Why?

```js
        setTimeout(() => {
          console.log('boom');
          this.setState({
            count: 22
          });
        }, 1500);
```

Every time state changes, `render()` gets called again. That's how it redraws the UI. So `setTimeout()` is getting called again. I'll prove it with a `console.log()`.

And we see it logging over and over. We don't see the change every time, because after that first change, we're just changing the value in state from `22` to `22` over and over.

```js
        setTimeout(() => {
          this.setState((state, props) => ({
            count: state.count + 1
          }));
        }, 1500);
```

Let's try basing the new state on the old state. Remember how to do that? We use a callback, because state updates can happen asynchronously. So instead of just passing the object into `this.setState()`, I give it a callback. The callback receives the previous state and the props as arguments. And I want the callback to return the object containing the changes to state. Since I'm using an arrow function, I can just wrap my return value in parentheses. The state property I'm changing is `count`, and I don't want to set it to `this.state.count + 1`, because that could be out of date. I want to use the `state` object that was passed into the callback. So: `state.count + 1`. Does this work?

Yeah, it does! Again, it changes roughly every 1.5 seconds, because `setTimeout()` runs again every time we render.

So this proves that the application does re-render when we update state. As we build less contrived examples, I think you'll begin to appreciate the power and convenience this affords. Because this _was_ a little contrived, right? Most of the time, we want to update state not just after a certain length of time, but in response to some action that the user took. Some _event_.

So next time, we'll talk about event handling in React. It's gonna be good! Until then, I'm Davey Strus. Haaaappy hacking!
