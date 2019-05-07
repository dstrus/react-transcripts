Hey, everyone. Welcome to Kenzie Academy. I'm Davey Strus.

We're editing an app that we created with Create React App. We've already added one component of our own—a stateless functional component. Let's add a stateful component this time. It will be a clock that updates every second. We'll also use lifecycle methods for the first time. We've talked about them, but not used them. How does that sound?

I'll call the new component `Clock`, so I'll add a `clock` folder inside `components`, and a file in that new folder called capital-C `Clock.js`. Since it requires both state and lifecycle methods, this component will need to be a class. You saw that I have a few steps that I always do right off the bat when I create a functional component, to make sure that I get a working component as quickly as possible. So it probably won't surprise you to hear that I have a similar process for class components. The specific steps are just a little different.

First, I import not just `React`, but also `{ Component }` from `'react'`. `React` is the default export from the `react` package, so it's not in curly braces, but `Component` is a named export, so it is in curly braces.

```js
import React, { Component } from 'react';
```

Next, I make my actual component, which is a class named `Clock` that extends `Component`. If I had only imported `React` and not `Component`, then I would just write `extends React.Component` instead of `extends Component`. But I'm in the habit of importing `Component`, so that this line is a little tidier.

```js
class Clock extends Component {

}
```

Now, before I forget, I make this my default export.

```js
export default Clock;
```

Now what else do I need to make this a complete component? I need a `render()` method, and that method needs to return some JSX. I'll just return a `div` with a class name that matches the name of my component. Inside that `div`, I'll just put the name of the component. That way, when I render this component, _something_ will show up on the page.

```js
render() {
  return (
    <div className="Clock">
      Clock
    </div>
  );
}
```

My component seems complete now, so I'm going to pop over to `App.js` and use it.

I need to import the component and then use it in my JSX. First the import...

```js
import Clock from './components/clock/Clock';
```

...and then the JSX. I'll jut put it right under `Welcome`, and the `Clock` component doesn't take any props.

```js
<Welcome name="Davey" />

<Clock />
```

I see the word "Clock" in the browser, so my new component is hooked up right. Now I can worry about what I actually want this component to contain.

Do you get why I do it this way? I don't want to spend a bunch of time building out a component, have it not work, and spend a bunch of time troubleshooting, only to discover that I misspelled "render" or forgot to export, or something like that. I want to make sure that I've got a working component first. Then I can worry about the details.

Now, let's see to those details. The `Clock` component will contain a clock that updates every second. We'll store a date object in state, update it every second, and display it in that `div`. If we want a date object in state, we'll need to set the initial state. Where do we do that? In the constructor!

We'll add a constructor to our class. As always with a class component, be sure your constructor receives props and passes them along to the constructor from the base class.

```js
constructor(props) {
  super(props);
}
```

Then we set the initial state. This is the one time that we set state via plain old assignment. We'll add a property called `date`, and we'll set the initial value to `new Date()`.

```js
constructor(props) {
  super(props);

  this.state = {
    date: new Date()
  };
}
```

OK, now we have a date object in state. Let's be sure we actually render it somewhere. Inside our `div`, let's add an `h2`, and print the date in there.

```js
<h2>It is {this.state.date.toLocaleTimeString()}.</h2>
```

Let's check that out. OK, it works. It doesn't update automatically though, does it? We haven't done anything to update state after it's set initially. So let's do that next.

We want to update state with the current date every second. Before we worry about doing it every second, let's make sure we have a method that updates state with the current date.

I'll add a method called `tick`, and all it will do is call `this.setState()`, and set the date to a new `Date` object.

```js
tick() {
  this.setState({
    date: new Date()
  });
}
```

Now we just need to be sure we call this every second. We can do that with `setInterval()`. The question is, where do we call `setInterval()`? We want to start updating it every second as soon as the component first renders. There happens to be a lifecycle method for just such occasions.

There are several of these so-called **lifecycle methods** that are invoked automatically at certain points in a component's life—for example, right after it's rendered for the first time, right after it's updated, or right before it is removed from the DOM. All you have to do is implement a method with the right name, and it will automatically run at the right time. That's because we inherit from `React.Component`. All the tricky stuff is in that base class. We just have to use the right methods names.

The lifecycle method that runs as soon as the component is rendered for the first time is called `componentDidMount()`, so we'll implement that method now. I like to put lifecycle methods right after the constructor. You don't have to do that, but I like to, so I can always see at a glance whether any lifecycle methods are present in a particular class.

Inside `componentDidMount()`, I'll call `setInterval()`. It will run `this.tick()`—I'll want to do that inside an arrow function, so the value of `this` gets bound correctly—and it will run it every second, so 1000 milliseconds.

```js
componentDidMount() {
  setInterval(
    () => this.tick(),
    1000
  );
}
```

Since we're calling `setInterval()`, we should also call `clearInterval()`. To do that, we'll need to save the ID that's returned by `setInterval()`. I'll just save it to a property of my class. It doesn't need to be in state. `this.timerId` is perfectly adequate.

```js
componentDidMount() {
  this.timerId = setInterval(
    () => this.tick(),
    1000
  );
}
```

`componentDidMount()` runs one time, when the component is first rendered, when it's first added to the DOM. Now we need a lifecycle method that runs just before it's _removed_ from the DOM. There is such a method, and it's called `componentWillUnmount()`. You'll notice that some of the lifecycle methods have names that include `Will` or `Did`. `Will` tells you that the method runs just _before_ something happens, and `Did` tells you that it runs right _after_ something happens. `componentDidMount()`, then, runs just _after_ the component _mounts_, or is added to the DOM. `componentWillUnmount()` runs just _before_ the component is _unmounted_, or is _removed_ from the DOM.

Let's implement `componentWillUnmount()` now. All it needs to do is call `clearInterval()` with the ID that we saved back in `componentDidMount()`.

```js
componentWillUnmount() {
  clearInterval(this.timerId);
}
```

Does our timer work? It sure does! There's not a great way for us to test our `clearInterval()` right now, because this component doesn't get unmounted until we close the entire page. Maybe that's something we can check out later on.

For now, we've added a stateful component to our existing app structure, and we've implemented a couple of lifecycle methods.

Next, we'll add a form to the page. There are a couple of ways to deal with forms in React, and we'll be exploring one of them. Until then, I'm Davey Strus. Haaaappy hacking!
