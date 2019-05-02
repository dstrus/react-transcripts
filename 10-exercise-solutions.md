Hey, everyone. Welcome to Kenzie Academy. I'm Davey Strus.

Let's look at some possible solutions to these exercises. There are many solutions. I'm just going to show one for each.

So first, prevent the counter from going below zero. That could happen when we decrement, right? So why don't we just put a safety check around that?

We _don't_ want to decrement it if it's already zero or smaller. So how about I just say, `if (this.state.count > 0)`? Only then do I call `setState()`.

```js
handleDecrement = (event) => {
  if (this.state.count > 0) {
    this.setState((state, props) => ({
      count: state.count - 1
    }));
  }
}
```

It would arguably be more correct to actually do this inside the callback for `setState()`, but for this app, this ought to be fine.

Now preventing the counter from going above twenty, that's something we do in `handleIncrement()`. And we'll just do the same sort of thing. `if (this.state.count < 20)`, only then do we call `this.setState()`.

```js
handleIncrement = (event) => {
  if (this.state.count < 20) {
    this.setState((state, props) => ({
      count: state.count + 1
    }));
  }
}
```

That ought to do it. Let's try it out. I cannot decrement, but if I get higher than zero, I can. It stops when I get to zero. And increment stops when I get to twenty. It starts working again if I go back down. Great! That seems to do it.

Next, turn the counter red when it's ten or higher. This involves styling the component, which is something we haven't talked about yet. I gave you a link to the documentation, [Styling and CSS](https://reactjs.org/docs/faq-styling.html). There are lots of ways to do it. Probably the simplest is to pass a string as the `className` prop. So we use `className` instead of `class`, because we're using the JavaScript property `className`, not the HTML attribute `class`. And you can just pass a string in there, and it will apply that class.

So let's make a CSS class to turn it red. These are very special counts that we're turning red, so I'll name my class `special`.

```css
.special {
  color: red;
}
```

Good enough. Now I only want to apply this style if the count is ten or higher. I'll just do this inside my `render()` method. I'll have a variable for the `className`, and if it's not ten or higher, I'm just not going to have a class, so I'll initialize it as an empty string...

```js
let countClassName = '';
```

...and if `this.state.count` is greater than or equal to ten, I'll reassign `countClassName` to `'special'`.

```js
if (this.state.count >= 10) {
  countClassName = 'special';
}
```

Now I need to apply this class to my count, so I need to wrap my count in some sort of element. I'll just put a `span` around it. And I need to give it `className`. `className` just takes a string, but the string is a variable this time, so I'll need to use an expression, so curly braces instead of quotes. And I'll just put `countClassName` in there.

```js
<div>
  <button onClick={this.handleDecrement}>decrement (-)</button>
  <span className={countClassName}>{this.state.count}</span>
  <button onClick={this.handleIncrement}>increment (+)</button>
</div>
```

So if the count is less than ten, `countClassName` will be an empty string, and this will effectively be `className=""`. If it's greater than or equal to ten, `countClassName` will be the string `'special'`, and `className` will be set to the string `'special'`, and that is the name of our CSS class. Let's try it out.

All right, it turned red as soon as I got to ten. Now the next item was to turn it back to the original color if it got below ten. Well, we've already solved that. This solution actually knocked out both of those. Cool!

Now, allow the initial count to be set from a prop. All right. Let's call the prop `initalCount`. How do we pass that in? Down here in the JSX, when we actually render our element, we would put the prop as an attribute, right? So we'll just set it to anything other than zero. How about five?

```js
ReactDOM.render(
  <Counter initialCount={5} />,
  document.getElementById('root')
);
```

Notice that I put that in curly braces, because it is the _number_ `5`, not the _string_ `'5'`.

So how do I use it? Up in the constructor is where we set the initial count, right? Now our props are passed into the constructor as this argument, which we've named `props`. So I could just set that initial count to `props.initialCount`.

```js
constructor(props) {
  super(props);

  this.state = {
    count: props.initialCount
  };
}
```

We want to be sure if it's not passed in, however, that it starts at zero. So I could put `props.initialCount || 0`.

```js
  this.state = {
    count: props.initialCount || 0
  };
```

That way if `props.initialCount` is `undefined`, the left side of the `||` will be falsey, and it will check the right side, and we'll get `0`. What do you think? Get it? Let's try it out.

It started at `5`, and my buttons still work. Cool.

Now if I have this code up here in the constructor, but I don't pass the prop in—just take this out of the JSX...

```js
ReactDOM.render(
  <Counter />,
  document.getElementById('root')
);
```

...does it still start at zero? Yes, it does. Cool. Let's put that back, and what if I _did_ use the _string_ `'5'`? Are we sure that wouldn't work?

```js
ReactDOM.render(
  <Counter initialCount="5" />,
  document.getElementById('root')
);
```

Looks like it started at five. Increment... oh, `51`. Yeah, not what we meant, right? That's what we get when we take the _string_ `'5'` and add `1` to it. So yeah, let's keep that the _number_ `5`.

```js
ReactDOM.render(
  <Counter initialCount={5} />,
  document.getElementById('root')
);
```

Very good. So to make it take a prop, we just need to pass that prop along, and then use it somewhere. In the constructor, we use it with `props.whateverThatPropNameIs`. Elsewhere in the class, we would use `this.props`, but in the constructor, it's just `props`.

Finally, add a button to reset the counter, and only show it if the counter's been modified. We'll start with just adding the button. I'm going to go ahead and add it in a `div` underneath the other things, just because I want to. It'll say "reset".

```js
<div>
  <button onClick={this.handleDecrement}>decrement (-)</button>
  <span className={countClassName}>{this.state.count}</span>
  <button onClick={this.handleIncrement}>increment (+)</button>
  <div>
    <button>reset</button>
  </div>
</div>
```

All right, so there's the button. Let's make sure that's there... it's there. Now what's it supposed to do? Reset the counter to its initial value. All right, so we'll need a new method to act as the event handler. I'll put it after `handleDecrement()`. How about `resetCount()`?

```js
resetCount = (event) => {
  this.setState({
    count: this.props.initialCount || 0
  });
}
```

Good enough. And we're going to use public class field syntax, so we'll set it to an arrow function. And what's it going to do? It's going to set state, and it's going to set it back to the initial value. So `this.setState()` is how we have to do it, and we shouldn't need a callback this time. We should just be able to pass in the object, because we're not basing it on the current state. So the value of `count` we're going to set back to... what? The initial value, which from here would be `this.props.initialCount || 0`. Right? Set it back to its initial count or zero. From this method, the props are in `this.props`.

Now we just need to make the button actually call that. So add our `onClick`—camelCased, of course—curly braces, `this.resetCount`, no parentheses.

```js
<div>
  <button onClick={this.resetCount}>reset</button>
</div>
```

Let's try it out. All right, it changes it back to `5`. Cool.

Now we only want to show this button if the count has changed from its initial value. There are lots of ways of doing this. This is called **conditional rendering**. That's another thing you could look up in the documentation: [Conditional Rendering](https://reactjs.org/docs/conditional-rendering.html). There are lots and lots of ways to do this. We'll just look at one. We'll do it inline using `&&`.

You know the trick to do an `if` using `&&`? Let's try it out. We can put any JavaScript expression in the middle of our JSX by surrounding it in curly braces, so let me put curly braces around this `div`.

```js
<div>
  <button onClick={this.handleDecrement}>decrement (-)</button>
  <span className={countClassName}>{this.state.count}</span>
  <button onClick={this.handleIncrement}>increment (+)</button>
  {
    <div>
      <button onClick={this.resetCount}>reset</button>
    </div>
  }
</div>
```

And what I'm going to do is put my condition—so, the circumstances under which I want to show it, I want to show it if the current count is different from the initial count—if it has ever _changed_. Well, we could say `this.state.count !== this.props.initialCount`, and that works as long as we had that prop, right? So this is the condition, and then I put `&&`, and then this is a multi-line JSX expression, so it needs to be in parentheses. Put parentheses here, and go ahead and indent this.

```js
  {
    (this.state.count !== this.props.initialCount) && (
      <div>
        <button onClick={this.resetCount}>reset</button>
      </div>
    )
  }
```

There we go. What this expression will do is check this condition. If it's `false`, that's the end, and this expression will render nothing. If the condition is `true`, however, it'll check the right side of the `&&`, which will be a JSX expression, so this expression inside curly braces will evaluate to this JSX, and we'll see the button on the page. See if this works.

OK, I don't see the button. I change the value, and the button shows up. I go back to my initial value, and it disappears again. Cool.

But if we didn't pass in the `initialCount` prop, this isn't going to work, is it? So what we could do it just set a variable up here.

```js
render() {
  const intialCount = this.props.initialCount || 0;
  // ...
}
```

Let's make sure it still works with an `initialCount`. Yep, the button still shows up and does what it's supposed to. But if I stop passing in the `initialCount`, what happens? Ah, the button's there still. That didn't quite do it. That's because I didn't change my `if`. It still compares it to `this.props.initialCount`. I want to compare it to my _variable_ `initialCount`—my local variable that I set up here.

```js
  {
    (this.state.count !== initialCount) && (
      <div>
        <button onClick={this.resetCount}>reset</button>
      </div>
    )
  }
```

Try that again. OK, no button. I change it, the button's there. I reset it, the button goes away. Cool. So that's that.

So there is some tricky stuff in here. We learned a few new things. We learned about conditional rendering—at least one way to do it. And we learned how to apply a CSS class name using the `className` prop.

I strongly encourage you to do some independent research on any of these topics that continue to trip you up, or about which you're simply curious.

Next we'll learn about Create React App, which is a great tool for getting a fully-configured, production-ready React app up and running very quickly. Until then, I'm Davey Strus. Haaaappy hacking!
