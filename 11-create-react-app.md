Hey, everyone. Welcome to Kenzie Academy. I'm Davey Strus.

I'd like to tell you about Create React App. Setting up a truly production-ready React app takes quite a bit of configuration. Learning how to configure a React app from scratch is almost as hard as learning React itself. Thankfully, there's Create React App.

Let's talk about _what_ Create React App is, _why_ we should care, and _how_ we actually use it.

First, what exactly _is_ Create React App? It's a command-line tool for scaffolding pre-configured React projects.

There are a number of React boilerplate projects you can find on GitHub to get you started, but Create React App is an official project from Facebook—the creators of React—and it's both flexible and powerful. All-in-all, it's a great way to get your app off the ground.

Now let's talk about _why_ this is even necessary. It seems like you just need to load three external scripts, and you're in business! Yeah, what we've been doing so far _works_, but we've had a couple of messages in our console the whole time.

> Download the React DevTools for a better development experience: https://fb.me/react-devtoolsYou might need to use a local HTTP server (instead of file://): [https://fb.me/react-devtools-faq](https://fb.me/react-devtools-faq)

The first is a message from ReactDOM that we ought to check out the React DevTools, and we'll need to actually run a web server before we can do that. And it turns out, there are a lot of other reasons we want to use a web server, even during development. OK, fair enough, but running a web server doesn't require _that_ much setup.

But that's not the only message, is it?

> You are using the in-browser Babel transformer. Be sure to precompile your scripts for production - [https://babeljs.io/docs/setup/](https://babeljs.io/docs/setup/)

The second one is not just a message, but an actual _warning_ from Babel that we really don't want to run Babel in the browser. We want to precompile our scripts. That's a little more work. And this is only the stuff that the included scripts are going out of their way to tell us about.

Create React App pre-configures Babel, and that alone gives us a lot. Babel includes support for JSX, as well as the very latest features of ECMAScript, well beyond ES6. Object spread syntax, async/await, class fields—all kinds of great stuff.

And there are a whole lot of other pre-compile tools like Babel that we could benefit from, and all sorts of best practices that Create React App encourages out of the box.

Want to use TypeScript? Gotta configure that! Unless you're using Create React App, that is.

Maybe you're all about type checking, but you're not into TypeScript. Want to use Flow? That's a static type checker for JavaScript, developed by Facebook. And guess what? Create React App includes support for that too.

OK, maybe just JavaScript with JSX is enough for you. You could still benefit from autoprefixed CSS.

You can type this...

```css
.App {
  display: flex;
  flex-direction: row;
  align-items: center;
}
```

...and it turns it into this:

```css
.App {
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-orient: horizontal;
  -webkit-box-direction: normal;
  -ms-flex-direction: row;
  flex-direction: row;
  -webkit-box-align: center;
  -ms-flex-align: center;
  align-items: center;
}
```

No more remembering all that `-webkit`, `-ms` business, all the different syntaxes, and which browser versions actually require those vendor prefixes. Create React App will take care of it for you, and it's turned on by default.

Create React App also comes with a fast, interactive unit test runner with built-in, zero-configuration coverage reporting.

There's also a development server with live-reloading that warns about common mistakes.

Create React App includes everything you need for a Progressive Web App, including an offline-first service worker and a web app manifest. You have to opt-in to the offline-first behavior, but it's as simple as changing one line of code. Check out the documentation to learn more.

And the build script bundles all your JavaScript, CSS, and images for production, with hashes and sourcemaps. It uses a module bundler called webpack, which can be pretty tricky to configure, but we don't have to worry about it. It's truly production-ready, right out of the box.

Opinions. Everybody's got one, right? Create React App is no exception. What do I mean when I say that Create React App is _opinionated_? Well, it comes with all these tools, and they're all preconfigured a certain way. The default configurations follow best-practices, so there's a good chance you'll be happy with what it gives you, but if you need to change the configuration, you can, as long as you're willing to go to a little trouble to do so. More on that later.

Whew! OK, that's a lot, and we're only scratching the surface here, folks. You don't need to know off the top of your head that Create React App supports all that stuff. Hopefully you're getting the picture that Create React App enables a ton of great features, and saves you a lot of configuration.

Now _how_ do you use it?

First, you need to install the command-line tool globally: `npm i -g create-react-app`.

```shell
$ npm i -g create-react-app
```

Then, you can actually create an app. Just type `create-react-app my-app`, assuming you want a project named `my-app`.

```shell
$ create-react-app my-app
```

It will automatically create the `my-app` project folder inside the folder where you run this command. It creates a bunch of files, installs dependencies, and even creates a Git repository. It warns that this might take a couple of minutes. I've sped it up in this video.

```shell
Success! Created my-app at /Users/dstrus/code/kenzie/react/projects/my-app
Inside that directory, you can run several commands:

  npm start
    Starts the development server.

  npm run build
    Bundles the app into static files for production.

  npm test
    Starts the test runner.

  npm run eject
    Removes this tool and copies build dependencies, configuration files
    and scripts into the app directory. If you do this, you can’t go back!

We suggest that you begin by typing:

  cd my-app
  npm start

Happy hacking!
```

At the end, it lists a number of commands you can run inside the project directory, beginning with `npm start` to start the development server. Wait a minute! "Happy hacking!"? That's my line!

Anyway, we can run `npm start` to start a development server from the project directory, but we're not yet _in_ the project directory, so let's get there. Then let's run `npm start`.

```shell
$ cd my-app
$ npm start
```


```shell
Compiled successfully!

You can now view my-app in the browser.

  Local:            http://localhost:3000/
  On Your Network:  http://192.168.1.2:3000/

Note that the development build is not optimized.
To create a production build, use npm run build.
```

Not only does it start a server on port 3000, it opens up the app in a new browser tab. Cool! Go ahead and kill your server with Ctrl C, and let's have a look at what Create React App actually did.

Let's open the project folder in VS Code or another editor or IDE and have a look.

```shell
my-app
├── node_modules
├── package-lock.json
├── package.json
├── .gitignore
├── README.md
├── public
│   └── favicon.ico
│   └── index.html
│   └── manifest.json
└── src
    └── App.css
    └── App.js
    └── App.test.js
    └── index.css
    └── index.js
    └── logo.svg
    └── registerServiceWorker.js
```

Create React App created this entire folder structure, including a `package.json` file. Notice that there's already a `node_modules` folder and a `package-lock` file. Create React App actually runs `npm install` as part of the setup process, so all your dependencies are already there. Where's does that list of dependencies come from? `package.json`!

```json
  "dependencies": {
    "react": "^16.8.6",
    "react-dom": "^16.8.6",
    "react-scripts": "3.0.0"
  },
```

The `package.json` file lists surprisingly few dependencies. Just React, ReactDOM, and `react-scripts`. `react-scripts` is where a lot of Create React App's magic happens. That one dependency has many, many dependencies of its own. Create React App hides all of that from us. All of these dependencies are already installed in the `node_modules` folder.

```json
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  },
```

We were able to run `npm start`, because there's a start script defined in the `scripts` section of `package.json`. Like all the other scripts listed, the `start` script actually runs `react-scripts`, which is that other dependency listed besides React and ReactDOM.

```json
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  },
```

`eject` is an interesting one. I mentioned earlier that you can configure all of the hidden dependencies if you're willing to go to some trouble. That's what this is about. If you run `npm run eject`, it will expose all of those configuration files, and your `package.json` file will get a whole lot bigger too. This is a one-way operation. Once you've ejected, you can't get your app back to this simpler setup. I encourage you to try it on a test app, just to see what it looks like, but don't do it on a real project unless you have a really good reason.

```json
  "eslintConfig": {
    "extends": "react-app"
  },
```

`package.json` also includes an `eslintConfig` section. ESLint is an excellent code analysis tool, and it's preconfigured for us.

```json
  "browserslist": {
    "production": [
      ">0.2%",
      "not dead",
      "not op_mini all"
    ],
    "development": [
      "last 1 chrome version",
      "last 1 firefox version",
      "last 1 safari version"
    ]
  }
```

Create React App 3 added support for `browserslist`, which is used by Babel and some other included tools to decide which browsers to target in your build. So that's it for the `package.json` file.

```shell
my-app
├── node_modules
├── package-lock.json
├── package.json
├── .gitignore
├── README.md
```

There's also a .gitignore file, so stuff like `node_modules` that shouldn't be committed to the repository, won't be. Remember, Create React App automatically creates a Git repository, and it even makes the initial commit.

Create React App also comes with a README, which contains some handy reference material, including a link to Create React App's documentation, which happens to be excellent. Now let's drill into some of these folders.

```shell
├── public
│   └── favicon.ico
│   └── index.html
│   └── manifest.json
```

We'll start with the `public` folder. Stuff in the public folder is going to get served directly. You won't use it a whole lot. You _will_ usually want to replace the default favicon.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <link rel="shortcut icon" href="%PUBLIC_URL%/favicon.ico" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <meta name="theme-color" content="#000000" />
    <link rel="manifest" href="%PUBLIC_URL%/manifest.json" />
    <title>React App</title>
  </head>
  <body>
    <noscript>You need to enable JavaScript to run this app.</noscript>
    <div id="root"></div>
  </body>
</html>
```

`index.html` is probably exactly what you think it is. It's the public entry point to your application, and it already has that `#root` div where we'll render our React app. In fact, you may very well not need to do anything more to `index.html` other than change the title. In most cases, you don't even need to add stylesheets, because you'll be importing those in your JavaScript. Yes, you heard that right. With the default webpack configuration in Create React App, you can import CSS in your JavaScript. That may sound odd, but I think you'll find that it's pretty cool. If you want to know more about what's going on the `index.html` that they give you, be sure to read the comments in the file.

```shell
└── src
    └── App.css
    └── App.js
    └── App.test.js
    └── index.css
    └── index.js
    └── logo.svg
    └── serviceWorker.js
```

Finally, we have the `src` directory. This is where you'll be doing most of your work, as it's where all of your components will live. That includes not only your JavaScript files, but even your CSS and images. Let's look at some of these files.

```js
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import * as serviceWorker from './serviceWorker';

ReactDOM.render(<App />, document.getElementById('root'));

serviceWorker.unregister();
```

`index.js` is JavaScript's entry point into your application. Its main job is to run `ReactDOM.render()` to replace that `#root` div with a React component. It's also important that it import React itself, because JSX won't work without it. JSX is shorthand for `React.createElement`, after all. It also imports a stylesheet, and you _could_ put all of your styles in that file, `index.css`, and it would work fine. But we can organize things a little better than that.

```js
ReactDOM.render(<App />, document.getElementById('root'));
```

Notice that it's rendering the `App` component, but the `App` component isn't actually in this file. It's being imported. This is the norm. You'll generally put each component in its own file. Let's look at that component.

```shell
└── src
    └── App.css
    └── App.js
    └── App.test.js
```

There are three `App` files: `App.css`, `App.js`, and `App.test.js`. They're all named `App`, because they're all related to the `App` component. The CSS file contains styles specific to that component, the JS file contains the component itself, and the `.test.js` file contains a unit test for the component. Let's just look at the JS file.

`App.js` contains the `App` component. It's extremely common to call your root component `App`, so Create React App just does it out of the box. It's also conventional to give the filename exactly the same name as the component, capital letters and all. This makes it really easy to spot which files contain components.

```js
import React from 'react';
import logo from './logo.svg';
import './App.css';
```

This file imports React itself. It's a functional component, so it doesn't need to extend the `React.Component` class. It needs React, because it's using JSX. Next, it imports an image and a stylesheet. Still seems a little odd, doesn't it?

We'll revisit that image in a minute. The idea behind importing the stylesheet is that you can have a stylesheet containing only the styles for _this_ component. Then you just import it into that component's JavaScript file, and webpack bundles the CSS alongside everything else. Note that there's nothing actually scoping this CSS file to this component. It's still possible to put styles in there that impact other components, so you'll need to take care when writing your selectors to prevent that.

```js
function App() {
  return (
    <div className="App">
      <header className="App-header">
        <img src={logo} className="App-logo" alt="logo" />
        <p>
          Edit <code>src/App.js</code> and save to reload.
        </p>
        <a
          className="App-link"
          href="https://reactjs.org"
          target="_blank"
          rel="noopener noreferrer"
        >
          Learn React
        </a>
      </header>
    </div>
  );
}
```

Looking at the actual component, it's a pretty straightforward functional component returning some JSX. But look at that `img` tag.

```js
<img src={logo} className="App-logo" alt="logo" />
```

First, it's self-closing, because empty elements must be self-closing in JSX, but look at the `src` attribute. Instead of a string value pointing at the path to the image, it has a JavaScript value—curly braces, rather than quotes—pointing at the imported image. This is how you use imported images. For bitmap files under 10 KB, it actually returns a data URI instead of a path, so the image is truly embedded in the JS and it saves you a roundtrip to the server. If it's over 10 KB, it puts in the appropriate path. Either way, it all happens seamlessly, and you don't have to worry about it.

Next time, we'll start to customize the app, and you'll begin to get a better feel for how to really make use of Create React App. Until then, I'm Davey Strus. Haaaappy hacking!
