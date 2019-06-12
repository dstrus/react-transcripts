Hey, everyone. Welcome to Kenzie Academy. I'm Davey Strus.

Earlier, we talked about **routing** as one of the fundamental features of single-page apps. In short, routing keeps our UI in sync with our URL. With routing, you can navigate between components as though they were separate pages. The URL even updates, and the back button works as expected. But you never technically reload the entire page. You're just swapping out a portion of the page. Let's learn how to do it.

We'll update our app so that we can navigate between the different components.

Routing is not built into React. We need a separate library for that. There are a few options, but React Router is the most popular, and it's pretty great. React Router comes in a few flavors, but for the web, we want React Router DOM.

We need to actually install it.

```js
$ npm i react-router-dom
```

Unless you're using a very old version of `npm`, this should not only install `react-router-dom` in your `node_modules` folder, it should also add it to your dependencies in `package.json`. Let's confirm that.

```json
  "dependencies": {
    "react": "^16.8.6",
    "react-dom": "^16.8.6",
    "react-router-dom": "^5.0.0",
    "react-scripts": "3.0.0"
  },
```

Sure enough. It's there. If you _don't_ see `react-router-dom` listed in your dependencies, then you have an older version of `npm`, and you need to use the `-s` command line switch when you run `npm i`. So, `npm i -s react-router-dom`, and then you should see it show up here.

To use React Router, we need to wrap our `App` component in another component that we import from `react-router-dom`. In `index.js`, I need to import `BrowserRouter` from `react-router-dom`. Notice that `BrowserRouter` is in curly braces, because it's a named export from the package.

```js
import { BrowserRouter } from 'react-router-dom';
```

You could also import just plain `Router` from `'react-router'` instead of `BrowserRouter` from `'react-router-dom'`, but that doesn't include any of the bindings to the DOM. In other words, it won't handle the browser's history API for you automatically, and that's a big part of what we want out of our router, so we will use `BrowserRouter`.

Now I'll nest the `App` component inside the `BrowserRouter` component. Having done that, let's look in the dev tools. I'll open them up with "Inspect" and switch to the "React" tab. I see a few new things in my list here—not just `BrowserRouter`, but also `Router` and `Context.Provider`. I guess it worked! Cool.

Now the router isn't doing us a whole lot of good, because we haven't defined any routes. React Router allows us to render specific components based on which routes match (or don't match) the current URL path. Let's define some routes!

I'll put routes in `App.js`. That requires importing `Route` from `react-router-dom`. I like to put all of the imports from external packages together, above the imports that come from other files in this project.

```js
import { Route } from 'react-router-dom';
```

And `Route` is a component. I'm going to get rid of all these components inside my `div` and put my routes there. We define our routes by making instances of the `Route` component with different props. Here's just about the simplest possible example:

```js
<Route path="/clock" component={Clock} />
```

So I've passed it two props, telling the component two things: First, use this route when the URL matches the path I've provided, `/clock`; and second, when it does match, render the `Clock` component. That's it. That's my route. A path to match, and a component to render. It can be as simple as that.

Right now, our `App` component looks empty. What if we manually change our URL to `localhost:3000/clock`? There we go! Now that route is active, because the URL matches the path we provided, so the `Clock` component appears.

Here's something you might not expect. If we make the URL `localhost:3000/clock/time`, for example... it still renders the `Clock` component. We can put _slash-anything_ after the matching path, and it still considers it a match. If you don't want that, add one more prop to the route: `exact`. Now `/clock/time` doesn't work, but if we hit the back button to go back to `/clock`, then that route matches again. I don't actually mind if it's inexact, so I'll take that prop off again.

Now let's go back to `/clock` and poke around in the dev tools. Switch to the React tab, and let's highlight the `Clock` component. Notice anything? It's now receiving three props: `history`, `location`, and `match`. We're not going to get into what those are for just yet, but when you set up a route like this, React Router always passes these three props to the components that it routes to. I'll go ahead and close the dev tools.

All right. We could use a route for `Contact`. It's going to be very similar. We'll use the `Route` component, pass it the path `/contact`, and the component `Contact`.

```js
<Route path="/contact" component={Contact} />
```

It's important that I've imported these components that I'm routing to, by the way. This prop value is a reference to the actual component. It's not just a string with the component name, right?

All right, let's try this. Manually navigate to `/contact`, and our `Contact` component appears. Cool. Hit _back_, and it goes away. Seems like we ought to see the `Welcome` component, doesn't it? Why don't we make `Welcome` render when the path is just the root, `localhost:3000`.

I'll list this route first. Give it a path of just `/`, and the component `Welcome`.

```js
<Route path="/" component={Welcome} />
```

Not quite perfect, is it? What's the problem? Well, `Welcome` expects a prop, doesn't it? The `name` prop. And we didn't pass that in. We just told React Router to render that component. We didn't tell it to pass anything along. If we open up the dev tools, I bet we'll find that it passed in those same three props it always passes in. Inspect, switch to the "React" tab, find `Welcome`... yep, those are there, but we don't have a `name`. I'll go ahead and close the dev tools.

Something else to note: What if we go to one of our other paths? Let's try `/contact` again. The `Contact` component is there, but so is the `Welcome` component. Do we want that? What if we don't? How can we fix it? Just add the `exact` prop, right?

```js
<Route exact path="/" component={Welcome} />
```

Now `Welcome` is no longer there when we visit `/contact`. If we go back to the root though... it's still there. Now what can we do about this prop? Well, you can't pass in props of your own when you use the `component` prop on the `Route` component. That will always just render the component with those three props that React Router always sends. If we want to send props of our own, we have to use a `render` prop instead.

```js
<Route exact path="/" render={} />
```

The `render` prop receives a callback that returns JSX, and it will render whatever JSX that callback returns. Since we're writing the JSX ourselves, that means we can pass along whatever props we want. So let's add a callback, and have it return the `Welcome` component with a `name` prop. I'm going to reformat this line while I'm at it, because it's getting a bit long.

```js
<Route
  exact
  path="/"
  render={() => <Welcome name="Davey" />}
/>
```

Now save, and it works! Let's look in the dev tools. Inspect, "React" tab, find `Welcome`... and there's my `name` prop. Ah, but there's something _else_ missing now. Those three special props the React Router passes in are no longer there. Since we wrote the JSX ourselves, it only passed in the props that we told it to.

Well, guess what? Our callback receives a `props` argument. We can just pass those props along in our JSX using _spread attributes_. Remember several videos ago when I said you should look up how to use spread attributes? Well, if you didn't do it then, you might want to do it now. It's a great way to pass along the properties of an object as individual props. Super handy for just passing along all of the props that a component has to one of its children.

Now if we find `Welcome` in the dev tools, we'll see that those three props are back, alongside the `name` prop that we added. Groovy. We can close the dev tools now.

So we have three working routes. But to switch between them, we have to manually change the URL. That doesn't seem ideal. Let's add a `Navigation` component with some links for switching between those routes.

We'll create a new `navigation` folder, and inside there, we'll add a new file, capital-N `Navigation.js`. This can be a functional component, so I'll import `React` from `react`, make a function named `Navigation` that receives `props` as an argument. It'll return some JSX, a `div` with a class of `Navigation`, containing the word Navigation, just so there's something on the page when I first hook it up. And then I'll export it.

```js
import React from 'react';

function Navigation(props) {
  return (
    <div className="Navigation">
      Navigation
    </div>
  );
}

export default Navigation;
```

Back in `App.js`, I'll import this and put it in the JSX just before my routes. First the import.

```js
import Navigation from './components/navigation/Navigation';
```

Then the JSX: `<Navigation />` with no props.

```js
<Navigation />
```

And the word "Navigation" appears on the page, so I hooked it up right. So what do we actually want in there? We want links, but we're not going to use plain old `a` tags. That would result in full page refreshes, which is not what we want. React Router gives us a `Link` component that does what we want, so let's import that. Import `Link`—that's a named export—from `react-router-dom`.

```js
import { Link } from 'react-router-dom';
```

It's easy to use. It just has a `to` prop that acts more or less like the `href` prop. So `<Link to="/">Home</Link>` to go the `Welcome` component. Let's go ahead and put these three links in a list. I'll make `ul` with three `li`s. Inside each `li`, I'll put a `Link` with a `to` prop. The first one goes home, the second one goes to the clock, and the third goes to contact.

```js
<ul>
  <li><Link to="/">Home</Link></li>
  <li><Link to="/clock">Clock</Link></li>
  <li><Link to="/contact">Contact</Link></li>
</ul>
```

There we go! Let's try them out. I'm switching between the components, the URL is changing, the back button still works, but I'm never doing a full page refresh. Excellent!

That's a quick overview of React Router. It's a pretty powerful library, and it can do a lot more tricks than we've explored here. You should explore some of those on your own. So next, I'll ask you to do a little exercise that may require you to dig a little deeper into React Router.

Until then, I'm Davey Strus. Haaaappy hacking!
