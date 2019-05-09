Hey, everyone. Welcome to Kenzie Academy. I'm Davey Strus.

Let's see if we can figure out this exercise.

First, we want to be able to go to `/welcome/Something`, some name, and have that name show up here, in the `Welcome` component. So we said we're going to need a new route that take us to the `Welcome` component besides the one here for the root path. So let's add that, and we said that the path should be `/welcome/:name`, where that `:name` is a URL param.

```js
<Route path="/welcome/:name" />
```

In this case, we might be able to get away with just saying `component={Welcome}`, because we're going to render the name out of the URL instead of the name from the prop, so we don't even need to pass the prop in. Let's try that.

```js
<Route path="/welcome/:name" component={Welcome} />
```

OK, how does this look? All right, so then in that component, `Welcome`, we change it so that if that param is there, we render it instead of the name from `props`. So let's make a variable up here, `const name`, and if `props.match.params.name` is there, that's what we want. If it's not, we want `props.name`.

```js
const name = props.match.params.name || props.name;
```

And then down here, we will render our variable, `name`, which will be one of the two things.

```js
<div className="Welcome">
  Welcome, {name}!
</div>
```

Let's see how this does. First of all, here we are on the root route, and it still works. It still shows "Davey", so our _or_ up here did work. It didn't cause a problem with `props.match.params` not being defined or something like that. Since we're still getting here with those three props intact, `match`, `history`, and `location`, `props.match` is defined, and `props.match.params` will always be defined if `props.match` is defined. So we're not going to have a problem with trying to access a property of `undefined` or something like that. So, this works.

Now, does it work when we go to `/welcome/Nicole`? Yes. "Welcome, Nicole!" It works. Try "Miles"... that works. "Eric"... that works. Excellent. If we go back to the root, it defaults to "Davey". All right, great. That's part one!

Part two was to use `Switch`, which is another component from `react-router-dom` to render a 404 component. So let's make that component first. So in my `components` folder, I'm actually going to call it `NoMatch`... nah, let's call it `404`. No reason we can't. And then create a new file called `404.js`. OK? And there doesn't need to be much to this. It can be a stateless functional component, so `import React from 'react'`.

```js
import React from 'react';
```

It's just a function. Now we can't name a function `404`, can we? So how about `NoMatch`? Yeah, we're going to need to go back to that name.

```js
function NoMatch(props) {

}
```

Very good. It will return some JSX. I'll make a `div` with the class `NoMatch`. And we'll just say, "Not found!" Then `export default NoMatch`.

```js
import React from 'react';

function NoMatch(props) {
  return (
    <div className="NoMatch">
      Not found!
    </div>
  );
}

export default NoMatch;
```

Let's go ahead and rename this thing: `NoMatch.js`. For the folder, I will put `no-match`. OK.

I'll import that in here.

```js
import NoMatch from './components/no-match/NoMatch';
```

And for the moment, I'll just throw it down here, just to be sure that it does work.

```js
<NoMatch />
```

There it is. It says "Not found" over here, so my component is wired up right, but that's not what we actually want to do with this, is it? We want to use `Switch` from React Router, and show that only if it doesn't match any of the other routes. So let's make sure we import `Switch` alongside `Route`.

```js
import { Route, Switch } from 'react-router-dom';
```

And it works like a `switch` statement. Let's just wrap all of these routes inside a `Switch` component...

```js
<Switch>
  <Route
    exact
    path="/"
    render={(props) => <Welcome {...props} name="Davey"/>}
  />
  <Route path="/welcome/:name" component={Welcome} />
  <Route path="/clock" component={Clock} />
  <Route path="/contact" component={Contact} />
</Switch>
```

...and what that does is match the first route that it matches and then stops looking. As soon as it finds a matching route, it no longer looks at the rest of the routes in that switch. So it's never going to render more than one of these. Right now, we could be rendering more than one of them. So we just need to be sure that at the very end we have a default. The way this works, if you have a route with no path, then that matches _everything_. So we'll just say `<Route component={NoMatch} />`.

```js
<Switch>
  <Route
    exact
    path="/"
    render={(props) => <Welcome {...props} name="Davey"/>}
  />
  <Route path="/welcome/:name" component={Welcome} />
  <Route path="/clock" component={Clock} />
  <Route path="/contact" component={Contact} />
  <Route component={NoMatch} />
</Switch>
```

OK, right now we're at "Welcome, Davey!" Go to `/clock` and get `Clock`. Go to `/contact` and get `Contact`. Go to `/pancakes`, and we get "Not found!" Excellent!

You could do these slightly differently, but this is the general idea. Next, we're going to do a lab. Until then, I'm Davey Strus. Haaaappy hacking!
