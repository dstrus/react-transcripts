Hey, everyone. Welcome to Kenzie Academy. I'm Davey Strus.

It's time to introduce JSX. JSX is a syntax extension to JavaScript that gives us a nice, straightforward way of describing the UI for React applications.

For most of this video, I'm just going to be showing you stuff. You don't need to code along until the very end.

```js
const element = <h1>Hello, React!</h1>;
```

Here's JSX. It looks like somebody got their HTML in my JavaScript. This struck me as incredibly bizarre the first time I saw it. The bit that looks like HTML isn't really. It's a JSX element, and it's just syntactic sugar for `React.createElement()`.

```js
const element = <h1>Hello, React!</h1>;
const element = React.createElement('h1', null, 'Hello, React!');
```

That's what's really happening here, but the HTML-like syntax is a whole lot friendlier. It's easer to read _and_ write. This becomes especially apparent when elements have children.

```js
const element = <div><h1>Hello, React!</h1></div>;
```

Here's a DIV with an H1 inside. Piece of cake with JSX. Without JSX?

```js
const element = React.createElement('div', null,
  React.createElement('h1', null, 'Hello, React!')
);
```

You have to call `createElement()` for both elements, and pass the child element as an argument to the `createElement()` call for the parent. Yuck! This gets out of hand pretty quickly.

```js
(<header>
  <h1>Welcome to my site</h1>
  <ul>
    <li><a href="home">Home</a></li>
    <li><a href="about">About</a></li>
    <li><a href="archives">Archives</a></li>
    <li><a href="contact">Contact</a></li>
  </ul>
</header>)
```

Here's some JSX that goes a couple levels deep. Without JSX?

```js
React.createElement('header', null,
  React.createElement('h1', null, 'Welcome to my site'),
  React.createElement('ul', null,
    React.createElement('li', null,
      React.createElement('a', {href: 'home'}, 'Home')
    ),
    React.createElement('li', null,
      React.createElement('a', {href: 'about'}, 'About')
    ),
    React.createElement('li', null,
      React.createElement('a', {href: 'archives'}, 'Archives')
    ),
    React.createElement('li', null,
      React.createElement('a', {href: 'contact'}, 'Contact')
    )
  )
)
```

Horror beyond imagination!

```js
(<header>
  <h1>Welcome to my site</h1>
  <ul>
    <li><a href="home">Home</a></li>
    <li><a href="about">About</a></li>
    <li><a href="archives">Archives</a></li>
    <li><a href="contact">Contact</a></li>
  </ul>
</header>)
```

Here's the JSX version again. Notice that I have parentheses around it. Multi-line JSX expressions must be enclosed in parentheses. Other than that, it just looks like HTML, right? JSX is convenient in part because you get to write something that looks an awful lot like the HTML that it will eventually render. Just remember that it isn't really HTML. It's just JavaScript dressed up like HTML—or, really, XML.

Another important rule: A JSX expression must return a single element and its children, if any. It can have as many children as you want, but there must be a single root element to the JSX expression. Otherwise, it would be the equivalent of trying to put two calls to `createElement()` in a row and calling it one expression. That doesn't work.

```js
const element = <p>I'd like {4 + 3} pancakes.</p>;
```

Check this out. I've put an expression inside curly braces. That works! The expression will be evaluated, and you'll see "7" in the rendered HTML. And yeah, you can put any expression inside curly braces in JSX, and it will be evaluated.

```js
const name = 'Davey';
const element = <p>Welcome, {name}!</p>;
```

Stick a variable in there. Enjoy yourself. So it may remind you of a templating language, but JSX comes with the full power of JavaScript. And because it's JavaScript, there are some things to watch out for.

```js
const element = <a href="about">About</a>;
```

This element has what looks like an attribute—`href`. In JSX, these are called _props_. We'll talk about props in detail later, but I want to point out something that might trip you up if you're not careful. Because JSX is closer to JavaScript than HTML, the prop names that correspond to common HTML attributes use the camelCased JavaScript property names.

```js
const elements = <div className="attendees">Me</div>;
const input = <input type="text" tabIndex="3" />;
```

For example, we use `className`—camelCased—instead of `class`, just like we do when accessing that property of an element with JavaScript. `tabIndex` is still `tabIndex`, but it's camelCased too. Say, you see something else funny about that `input` example?

```js
const input = <input type="text" tabIndex="3" />;
```

JSX follows the rules of well-formed XML, which means, among other things, that all tags must be closed. For empty elements like `input` or `img` that don't ordinarily have closing tags, they become self-closing tags. Just put a forward slash immediately before the closing angle bracket.

You don't have to use JSX... if you enjoy suffering. But it really isn't common to use React without JSX, and JSX does offer some nice features in addition to the convenient syntax. It can prevent injection attacks by escaping any values embedded in JSX before rendering them, and it also allows React to show us more helpful error and warning messages.

Let's modify our "Hello, React" app to use JSX instead of `createElement()`. Now is the time to start coding along with me. Open up your HTML from the last video.

Before we can use JSX, we need to include one more external script. Remember, JSX isn't standard JavaScript. It's an extension to JavaScript, so the browser doesn't understand it natively. We need a library like Babel that transforms our JSX into the actual `createElement()` calls.

```html
  <script crossorigin src="https://unpkg.com/react@16/umd/react.development.js"></script>
  <script crossorigin src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
  <script src="https://unpkg.com/babel-standalone@6/babel.min.js"></script>
```

We'll load Babel from a CDN. In a production application, you don't want to run Babel in the browser like this. You want to run it ahead of time as part of a build process, but we don't know how to do that yet.

Since we are running Babel in the browser, there's one more little change we have to make before we can actually use JSX. We have to change the script tag where we wrote our code to have the attribute `type="text/babel"`, because what we're writing inside that script tag is no longer standard JavaScript, and the browser will trip up if left to its own devices.

```html
  <script type="text/babel">
    const element = <h1>Hello, JSX!</h1>;
    ReactDOM.render(
      element,
      document.getElementById('root')
    );
  </script>
```

So now that we have Babel, we're free to use JSX. Just replace that `createElement()` call with what looks like an HTML `h1` tag. Notice that this isn't a string. It's not in quotes. This tag actually takes the place of `React.createElement()`. Babel will do the work of translating it from JSX to plain JavaScript, so we can take advantage of the convenience of JSX.

Let's check it out in the browser to make sure that this does, in fact, work. It almost seems too good to be true. Sure enough! It works. Notice the warning from Babel that we don't really want to use it in a browser in production.

Now you know a little about JSX—enough to appreciate its advantages, I hope.

We've been told that React apps are made up of _components_, so we should probably learn how to make our own components. That's next time. Until then, I'm Davey Strus. Haaaappy hacking!
