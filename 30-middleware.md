Hey, everyone. Welcome to Kenzie Academy. I'm Davey Strus.

Sometimes when an action kicks off, we need to do something besides just updating the store. Maybe we want to log something, maybe we need to make an asynchronous API call—it could be any number of things, but it's very common to need to do _something_ else, besides updating the store, when we dispatch an action.

Could we just do that stuff in our reducers? I guess we _could_, technically, we'd be violating the rules of pure functions, because then our reducers would have side effects. They'd be doing something other than just producing the predictable return value.

Could we do it in our action creators? I mean, I guess we _could_, but that doesn't feel right. Those are just supposed to return our action objects, not make API calls or anything like that.

That's what middleware is for. If you've done backend programming with Express, perhaps you're familiar with the basic idea of middleware. In Express, you might use middleware to do something with a request before it reaches the routing engine. Maybe you're messing with the headers somehow, or checking an authentication token.

In Redux, middleware gives us a way to intercept an action after it's dispatched, but before it reaches the reducer. Then we can do whatever we need to do, including messing with the action itself if we need to, and then decide whether to pass it on to the reducer.

The simplest example is probably a logger. Let's update our Redux-ified clock app, and whenever an action is dispatched, let's log the action type. We only have one action type in this app at the moment, so middleware may seem like overkill, but with middleware, we know it will scale. If we have twenty actions later, it will still totally work. Let's try it out.

I'll create a new folder called `middleware`, and make a file called `logger.js`.

The signature for middleware functions is kind of interesting. There's some currying going on, if you know your functional programming terms.

```js
function logger(store) {
  return function(next) {
    return function(action) {
      // actual logic goes here
    }
  }
}
```

We'll make our function, called `logger`. It will receive the `store` as an argument. And that function needs to return another function that receives an argument that we typically call `next`. This will, again, seem familiar if you've used middleware with Express. `next` is a function, and we use it to _chain_ middleware. It's a wrapper around dispatch, and calling it will move the action on to the next step in the process, whether that's another piece of middleware, or the reducer itself. Now _this_ function must also return a function that receives the actual _action_ as an argument. Inside _this_ function, you do whatever your middleware is actually supposed to do. So you'll have access to all three of those arguments—`store`, `next`, and `action` from inside this function.

This looks really gross, so let's rewrite this with ES6 arrow functions.

```js
const logger = store => next => action => {

}
```

Much better! Just remember `store`, `next`, `action`, in that order. Now, we could just do `console.log(action.type)`. To be sure that the reducer still runs, we need to run `next()`, passing in the action as an argument.

```js
const logger = store => next => action => {
  console.log(action.type)
  next(action)
}
```

Now we just need to register our middleware. We do that as an extra argument to `createStore()`.

Back in `index.js`, we need to import `applyMiddleware` from Redux, import our actual middleware, and then add an argument to `createStore()`. That will be a call to `applyMiddleware()`, passing in our actual middleware an an argument to that.

```js
const store = createStore(
  app,
  initialState,
  applyMiddleware(logger)
)
```

Let's see if it works! Open up our console... and yep, it's logging every one of those actions.

If we never call `next()` the reducer won't run. You can try that out if you want. That might be useful sometimes if your middleware needs to do some sort of validation. Do that, and you'll see that the action keeps getting dispatched every second, but the state never actually changes. OK, let's put that back.

Want to actually see the new state each time?

Let's make this a `console.group()`, and then after we run `next()`, and we know the reducer has actually run, we'll log the updated state.

```js
const logger = store => next => action => {
  console.group(action.type)
  next(action)
  console.log(store.getState())
  console.groupEnd()
}
```

Cool. I think you get the picture. This is really cluttering up our console, so let's _not_ apply this middleware right now.

What is our app already doing that could perhaps be done in middleware instead of wherever it's being done right now? What about managing the interval? What if we had `START_TIMER` and `STOP_TIMER` actions, and all the `Clock` component had to do is dispatch those actions. Middleware would then handle the `setInterval()` and `clearInterval()` stuff. And that timer ID is just a property on the component right now, which never felt great. We could keep that in our store. Let's try it out!
