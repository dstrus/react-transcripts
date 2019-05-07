Hey, everyone. Welcome to Kenzie Academy. I'm Davey Strus.

Now that we know how to create a new project with Create React App, let's actually add our own work to such an app. Let's code within the structure that Create React App generates for us. This will be our first time writing React with multiple components across multiple files, using ES6 imports and all of that good stuff.

We'll be creating a stateless functional component that uses a single prop. We've done that before, but never within this file structure. So let's get some good practice in and develop some good habits, as we grow accustomed to the structure that Create React App gives us.

If you want to do this within the `my-app` project that you generated last time, you may. It is good practice to go through the motions of starting a new project again, however, so I'm going to create a new app called `welcome`. Be sure you don't do this inside an existing project folder.

```shell
$ npx create-react-app welcome
```

Once again, I've sped up the video a bit. Now I'll change into my new project directory and run `npm start`, just to make sure that everything works.

```shell
$ cd welcome
$ npm start
```

That fires up a server on port 3000 and opens a new tab in my web browser. I see the animated React logo, so it looks like all is well. Notice that it says "Edit `src/App.js` and save to reload." If we make changes to our code, this browser window will reload automatically every time we save. We don't have to kill our server every time we make a change. We don't even need to hit _refresh_ in the browser. It does it for us.

So I'll go back to my terminal, but I'm not going to kill the server with `Ctrl C`. I'm going to keep it running and open a new terminal in the same folder, so I can continue working while the server is running. I'm going to keep the server running while I code. So I'll open up my code in Visual Studio Code. It doesn't have to be VS Code, but that's what I'm using. I'll get my editor and my browser window side by side.

"Edit `src/App.js` and save to reload." I'll do exactly that. Open up `App.js`, and there's a bunch of stuff in there that we didn't put there. I'm going to remove most of it. I'll leave the `div` with the class name `App`—very common pattern, by the way, making the root element of your component a div with a class name that matches the name of your component, including the capital letter—but I'll get rid of everything inside that `div`. I'll replace it all with the word "Hello", just so we can see it happen live in the browser.

```js
function App() {
  return (
    <div className="App">
      Hello
    </div>
  );
}
```

And there it is, centered at the top of the page.

My editor is now complaining about `logo`, the SVG that's imported at the top, because it's no longer being used. We removed the `img` tag, so we no longer need to import the file.

Now I've written a bit of CSS ahead of time to make this look slightly less boring as we build it. You can find it on the course web site. Just copy and paste it into `index.css` if you like. It's not much, but it gives us something more colorful to look at, and it puts the `.App` div right in the center of the page.

Now why does this work? Where is `index.css` being included? If you look at `index.html`, there's no `link` tag for it. It's not being imported in `App.js` either, although `App.css` is. It is being imported in `index.js`, which is the file that calls `ReactDOM.render()`. That's where this CSS file is imported. And importing it into a JavaScript file that's part of the app is all we have to do. Webpack will then include that CSS file in our bundle.

Back to `App.js`. It's pretty common for `App.js` not to do much other than import other components. That's what ours is going to do, as well. So let's add a second component.

I'm going to create a `components` folder inside my `src` folder. There's nothing magic about that name. You don't _have_ to put your components inside a `components` folder. I'm just doing it to keep things nice and organized.

Inside that folder, I'm going to create a new folder for each component that I create. Why would I do that? Again, you don't _have_ to, but depending on your app's structure, it can be helpful for the sake of keeping things organized. Notice that the `App` component really has three files: a CSS file, the JS file where the actual component is, and a test. When you have multiple files per component, it can be nice to keep each component in a separate folder. We're not actually going to create three files for each component in this example, but we'll follow that folder convention anyway.

I'm going to create a component called `Welcome`, so I'll create a folder inside the `components` folder, called `welcome`. And inside that folder, I'll create a file called `Welcome.js`, with a capital `W`. Notice that the `App` files were all named with a capital `A`. It's conventional to name your component files with the exact name of the component, capital letters and all. Once again, you don't _have_ to do that, but for heaven's sake, be consistent, and follow whatever conventions have been established by your team. This is a really common one. They follow it in AirBnB's React Style Guide, which is a popular one, so I'm going to do it. Just remember that when you import from such a file, it is case-sensitive.

In `Welcome.js`, I'm going to create a stateless functional component. There are a few things I do first every single time I create a new functional component. First, I import `React` itself.

```js
import React from `react`;
```

Then I create my component, which is just a function, and it always receives `props` as an argument.

```js
function Welcome(props) {

}
```

Next, before I forget, I export the component as the default export. It's very easy to forget to do that, so I always do it right after I add the component. At this point, my fingers almost do it automatically.

```js
export default Welcome;
```

Next, the only other thing I need to do before I have a complete component is to return some JSX from this function. I mentioned that it's very common to have the root element of your components be a `div` with a class name that matches the name of your component. I nearly always do this. I'll do it right now.

```js
function Welcome(props) {
  return (
    <div className="Welcome">
    </div>
  );
}
```

Now we have a complete component! It will render as an empty `div`, so I usually start it out with the name of the component inside that `div`.

```js
function Welcome(props) {
  return (
    <div className="Welcome">
      Welcome
    </div>
  );
}
```

This may seem silly, because that's not what we actually want to it to render in the end, but my first priority when creating a new component is to see that component on the page, so I know that everything is hooked up right. My component is formed correctly, my imports are all correct—that sort of thing, before I worry about what the component will actually contain.

So let's finish hooking this up. Let's include our `Welcome` component in our `App` component. I'll replace the word "Hello" with a JSX tag for the `Welcome` component.

```js
function App() {
  return (
    <div className="App">
      <Welcome />
    </div>
  );
}
```

Your editor will complain, as will the page in the browser. The development server we're running is nice enough to show us detailed error messages. `Welcome` is not defined. Why not? Because we didn't import the `Welcome` component in `App.js`. Let's do that now.

We are importing the default export from the `Welcome` file, so no curly braces around the name `Welcome`, and I am importing it with a capital letter, as is conventional for components. The path begins `./components`, because `components` is a subfolder of the folder where `App.js` is. Then _slash components_, _slash welcome_, _slash capital-W Welcome_.

```js
import Welcome from './components/welcome/Welcome';
```

Save, and the error goes away, and we see the world "Welcome" on the page. Inspect it, and you'll see that it's inside a `div` with the class `Welcome`. Great—our new component is rendering. Now we can worry about what we actually want in that component.

We just want a component that uses a prop. We'll just take a prop called `name`, and make sure we use it in the component. Before we change the component, we can update our JSX to pass the prop into the component. props in JSX look like attributes in HTML.

```js
<Welcome name="Davey" />
```

Back in `Welcome.js`, we're not yet doing anything with the `name` prop, so let's do that now. In my JSX, after the word `Welcome`, I'll add a comma, then a JavaScript expression inside curly braces. That expression will be `props.name`. `props` is what we named our argument, and `name` is the specific prop we're after, so it will appear as a property of that `props` object. I'll add an exclamation mark at the end, because I'm very excited to welcome myself.

```js
function Welcome(props) {
  return (
    <div className="Welcome">
      Welcome, {props.name}!
    </div>
  );
}
```

And it worked! The page now says, "Welcome, Davey!"

So that's how we use one component inside another. We import the new component, and we use it in the first component's JSX, passing along any props that we need.

Next time, we'll add a stateful component to this app. Until then, I'm Davey Strus. Haaaappy hacking!
