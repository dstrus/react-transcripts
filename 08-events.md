Hey, everyone. Welcome to Kenzie Academy. I'm Davey Strus.

When state changes, the UI is updated. We'd also like to be able to update state in response to actions from the user.

We need event handling! We've done this outside of React. We know that events can be any number of things.

Mouse clicks, taps on a mobile device, keypresses on a keyboard, mouse movement, the submission of a form. All of these are actions that the user can take, that we're then able to deal with through event handlers.

We can do things other than update state with event handlers, but updating state in the same component is a really common use case. Let's try to do that in our Counter app.

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

First of all, if you have that `setTimout()` business in there from last time, you can get rid of that.

```css
body {
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}
button {
  margin: 0.5rem;
}
```

I've also added just a little bit of CSS, because it annoys me that we have one tiny number in the extreme upper-left corner of the page. It's nothing fancy, but you're welcome to copy it if you want.

```js
  render() {
    return (
      <div>
        {this.state.count}
        <button>increment (+)</button>
      </div>
    );
  }
```

Now then, I'd like a button that will increment the counter every time I click it. All right. I'll go ahead and add the button to the DIV in my `render()` method. My JSX expression is now going to take up multiple lines, so I need to wrap it in parentheses. Now add the button... all right. There's my button. Load it up in the browser, and you'll see that the button doesn't do anything at this point. We want it to add 1 to that count. Now we need a method to call when it gets clicked. When you have a class component—a stateful component—your event handlers will usually be methods on the class, so I'll add a method after the constructor and before `render`. I always like to keep `render` the last method in the list, just so it's always easy to find, as it's the one method that always has to be there in a class component. There's no technical reason it needs to be last.

```js
  handleIncrement(event) {
    this.setState((state, props) => ({
      count: state.count + 1
    }));
  }
```

I'll call the new method `handleIncrement()`, in keeping with my habit of naming functions and methods as verbs or verb phrases. As an event handler, it will receive an `event` argument. Now this happens to be a synthetic event that React creates, rather than the raw event from the DOM, but it follows the spec, with the properties and methods you're used to, so you shouldn't need to worry about that. Now what should this method do? Add one to the current value of `this.state.count`. We know we can't just do `this.state.count ++`. We have to use `this.setState()`. If we don't, React won't know that state has changed, and it won't re-render the page. We're going to update the count to be one more than its current value, so our new state is based on a value from the previous state. Because our new state depends on the previous state, we have to use a callback. That's because state updates can happen asynchronously, and the state could change between the time we call `setState` and when the update is actually applied. We want to be sure we're adding 1 to the value when it's actually applied. The callback receives `state` and `props`. We just need to return an object, so I'll wrap the object in parentheses. That returns it, since this is an arrow function. And we'll set `count` to `state.count + 1`. Not `this.state.count`. Just `state.count`. `state`, as opposed to `this.state`, is the argument that's passed into the callback. By using `state` instead of `this.state`, we're guaranteeing that we're using the state value from the time the update is actually applied. All right, now it's a method. But how do we make it an event handler?

```js
  render() {
    return (
      <div>
        {this.state.count}
        <button onClick={this.handleIncrement}>increment (+)</button>
      </div>
    );
  }
```

You set it up with an attribute in the JSX. It's the button that will get clicked, so we'll add an `onClick` attribute to that tag. Notice that it's camelCased. That's a change from the DOM, where it's all lowercase. The value isn't a string, but the actual function, so we use curly braces instead of quotes. Inside those curly braces we put the event handler: `this.handleIncrement`.

```js
<button onClick={this.handleIncrement}>increment (+)</button>
```

Notice that I did _not_ include parentheses at the end of the method name. JSX will evaluate whatever expression is inside the curly braces, and assign the result as the event handler.

```js
{this.handleIncrement()}
```

So if I put parentheses here, what would this expression evaluate to? With the parentheses, I'm invoking `this.handleIncrement`, so this expression would evaluate to whatever `handleIncrement` returns—which is a whole lotta nothing! So the event handler would get set to `undefined`. That's no good.

```js
button.addEventListener('click', handleIncrement)
```

It's just like using `addEventListener()` outside of React, or any other time you have a callback. You want to be sure to pass the function itself, not its return value.

```js
<button onClick={this.handleIncrement}>increment (+)</button>
```

Just remember, we're not the ones invoking the function. React is invoking it for us, later, when the button is clicked. So does this work?

Nope! It doesn't. Well, shucks. Do we have errors in the console? We sure do. "Uncaught TypeError: Cannot read property `setState` of undefined." Let's go back to the code.

```js
      handleIncrement(event) {
        this.setState((state, props) => ({
          count: state.count + 1
        }));
      }
```

We call `setState` in `handleIncrement`. "Cannot read property `setState` of undefined." So what is it saying is undefined? `this` must be undefined! But `handleIncrement` is a method! Ah yes, but we didn't invoke it that way, did we? We let React invoke it for us when the event fired. This is a problem with using methods as event handlers outside of React, and it's a problem in React as well.

`this` is not automatically bound to the object when you use a method as an event handler. Outside of React, event handlers bind `this` to the event target by default. In React, `this` simply remains undefined. There are several ways we can address this.

```js
constructor(props) {
  super(props);

  this.state = {
    count: 0
  };

  this.handleIncrement = this.handleIncrement.bind(this);
}
```

First, we can actually call `bind()` on it. While you could do that down in the JSX, that will create a new binding every time you render, so you'd typically do it in the constructor. To the right of the equals sign, we're taking the method `this.handleIncrement` and binding the current value of `this`. That creates a new function, which is a bound version of the original function. Now we're assigning that new function to `this.handleIncrement`, so from now on, `this.handleIncrement` will refer to the bound version. Let's try it out!

Sure enough! That fixes it!

```js
<button onClick={(event) => this.handleIncrement(event)}>increment (+)</button>
```

Personally, I find calling `bind()` rather annoying, so I'd like to find another way. One alternative is to use an arrow function in our JSX. Be sure to pass that `event` argument on to `handleIncrement`. Arrow functions _are_ bound by default, and since we then use regular method invocation inside that arrow function to call `handleIncrement`, it ends up being bound. Let's double-check.

Yep, still good!

This has the same downside as calling `bind` from inside the JSX: Namely that a new callback gets created every time the component renders. So binding it in the constructor remains a little more efficient.

```js
<button onClick={this.handleIncrement}>increment (+)</button>
```

Now I'll show you my favorite way of solving this issue, and that is to use **public class field** syntax when creating the method. We can change the JSX back to just assign `this.handleIncrement`—no parentheses—as the event handler. And we'll just change our method definition.

```js
handleIncrement = (event) => {
  this.setState((state, props) => ({
    count: state.count + 1
  }));
}
```

Instead of a regular method definition, we'll assign the field `handleIncrement` to an arrow function. Using an arrow function binds the value of `this`, so we're good to go. Let's double-check that...

Yep. Still good.

```js
handleIncrement = (event) => {
  this.setState((state, props) => ({
    count: state.count + 1
  }));
}
```

This uses public class field syntax, which is a relatively new feature in ECMAScript. It has been supported in Babel for years now, and it's even supported natively in Chrome. So in all likelihood, it's safe for you to use. But if you're using a really old version of Babel, or you've configured Babel to have that feature turned off for whatever reason, you may need to use one of the other means of ensuring that `this` gets bound correctly. I'm going to stick with the public class field.

That's the general idea of handling events in React. It's not too terribly different from handling events outside of React. Shall we get a little more practice in? How about a _decrement_ button to go with our _increment_ button?

```js
handleDecrement = (event) => {
  this.setState((state, props) => ({
    count: state.count - 1
  }));
}
```

Let's make the event handler first. We'll use public class field syntax again, so assign `handleDecrement` an arrow function, with the `event` argument. The body is going to be very similar to `handleIncrement`, but we'll subtract 1, rather than adding it. So call `this.setState()`, give it a callback, receiving `state` and `props` as arguments. Return an object containing the key we want to change, which is `count`, and set it to `state.count - 1`. Very good.

```js
<button onClick={this.handleDecrement}>decrement (-)</button>
```

We have the method we're going to use. Now we need to make it an event handler. I'll put this button just _before_ the count. Add the `onClick` (camelCase). Set it to `this.handleDecrement`. Very good. That ought to do it. Shall we try it out?

Nice!

I actually forgot to remove that `bind()` from the constructor earlier, so that kind of defeats the purpose of showing you the alternative approaches. I'll remove it now, and you'll see that my increment button still works. Very good.

Things can get a little more complicated than this, naturally. You'll still have `preventDefault` and event targets and things like that to deal with sometimes. And forms are a whole _thing_ in React. But we'll get to that. For now, you should try some things on your own. Check back for a little exercise to give your brain a workout. Until then, I'm Davey Strus. Haaaappy hacking!
