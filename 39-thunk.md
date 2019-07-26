Hey, everyone. Welcome to Kenzie Academy. I'm Davey Strus.

You ready to get thunky?

If you've looked into React and Redux on your own, there's a good chance you've come across Redux Thunk, which is a piece of middleware for Redux. Now, we've used middleware of our own making and have some idea of the purpose of middleware, in general. But why do we need Redux Thunk, and as one [widely circulated blog post](https://daveceddia.com/what-is-a-thunk/) asks, what the heck is a _thunk_?

The term _thunk_ just refers to a function that is returned by another function. You've already used them, whether you know it or not. Remember `connect()`? When we create a Redux-connected version of a component—which we called a _container_—we have to call connect...

```js
connect()
```

...and then call the function that `connect()` returns, passing our component as an argument to _that_ function.

```js
connect()(MyComponent)
```

That second function is an example of a _thunk_, because it's returned by `connect()` and must be invoked separately, hence two sets of parentheses.

If this seems a tad confusing at this point, don't worry about it too much. Just know that the term _thunk_, in general, refers to a function that is returned by another function.

```js
function outerFunction() {
  return function innerFunction() {
    console.log('doing things!')
  }
}
```

In this example, `innerFunction()` is a thunk. You'll often see this with the ES6 arrow syntax.

```js
const outerFunction = () => () => {
  console.log('doing things!')
}
```

`outerFunction()` still returns that inner function, but the inner function has no name this time.

OK, that's cool and all, but we're talking about Redux middleware. Why do we need thunks in Redux? First, let's review the purpose of Redux middleware.

Sometimes when we dispatch an action, we want some other stuff to happen, besides just updating the store. Redux middleware can intercept an action and do stuff before actually passing it on to the reducer. What kind of stuff? Frequently, it's asynchronous stuff like making API calls, but it could be anything. We call these things _side effects_, because it's something other than returning the new state, which is all a reducer is supposed to do. Could you just perform those side effects in the reducer? Technically there's nothing stopping you, but our lives are much easier if our reducers remain _pure_, and performing side effects would break that rule.

So couldn't we perform the asynchronous operation in the component, and dispatch the action after it finishes? Again, technically, yes, there's nothing stopping you from doing that. But that's not really the component's job, and our components are much easier to reason about and test if they stick to dispatching actions and rendering.

Previously, we wrote our own middleware to perform side effects, like logging or running `setInterval()`. But think about our action creators for a moment.

```js
function doSomething() {
  return {
    type: 'DO_SOMETHING',
    payload: 'some data'
  }
}
```

They don't do much, right? They just return those very simple action objects. What if we just performed our side effects there? Maybe that doesn't feel quite right, but let's explore the possibility for a moment, just for funsies.

What if the side effect is something asynchronous, as it often is?

```js
const doSomething = () => (
  fetch('http://example.com/something.json')
    .then(response => response.json())
    .then(someJson => console.log('uhhh... what now?'))
)
```

We can't just dispatch a call to this action creator, because it returns a promise, rather than an action.

OK, well, we could make it not technically an action creator, and just dispatch a real action inside `then()`.

```js
const doSomething = () => (
  fetch('http://example.com/something.json')
    .then(response => response.json())
    .then(someJson => store.dispatch(someActionCreator(someJson)))
)
```

I mean... yeah, you could do that. But then you're just calling this function from your component, instead of dispatching an action that it returns, so it isn't clear when looking at your component that an action is getting dispatched. Plus, this pseudo-action-creator is now tightly coupled to that particular store, since you'd have to import the store there in order to run `dispatch()`.

So yeah, middleware feels like the right solution, but writing new custom middleware for every side effect you ever want to perform seems kind of ridiculous.

OK, what if an action creator could return a function that performs the side effects? We can then call _that_ function later, and maybe we'd give it `dispatch` as an argument, so it could dispatch another action. How would that look?

```js
const doSomething = () => dispatch => (
    fetch('http://example.com/something.json')
      .then(response => response.json())
      .then(someJson => dispatch(someActionCreator(someJson)))
)
```

There's our action creator. Then in the component... hmm. Well, we could call this function, then add a second set of parentheses and pass `dispatch` in there... but we still can't actually dispatch this action creator, because... well, it's not _really_ an action creator, is it? It doesn't return an action. It returns a function.

OK, so what if we just changed the rules for action creators, so that they can return _either_ an action _or_ a function, and if it's a function, that function—that _thunk_—gets called _automatically_?

That's exactly what Redux Thunk does: If your action creator returns a function instead of an action, Redux Thunk runs that function automatically, passing in `dispatch` and `getState`, so we can use those inside that function. That's what Redux Thunk does, and that's _all_ it does. The entire library is 14 lines, and that's including five lines that are either blank or contain only a single curly brace. Heck, the first release was a grand total of six lines. Redux Thunk is a tiny library, but it does something very useful, giving us a place to perform side effects while keeping our action creators and reducers nice and pure, and saving us the trouble of writing custom middleware every single time we have a new side effect to perform.
