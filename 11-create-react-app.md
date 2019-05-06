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

All you have to do is type `npx create-react-app my-app`, assuming you want a project named `my-app`.

```shell
$ npx create-react-app my-app
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

Not only does it start a server on port 3000, it opens up the app in a new browser tab. Cool! When you're finished, go ahead and kill your server with Ctrl C.

Next time, we'll open this project up in an editor and take a look at the files and folders that Create React App actually generated. Until then, I'm Davey Strus. Haaaappy hacking!
