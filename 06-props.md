Hey, everyone. Welcome to Kenzie Academy. I'm Davey Strus.

We've made our first component, and we've seen that there are two ways of creating components: As class components, or as stateless functional components.

We know that components take inputs called **props**, and they output a React component. We know how to do that last part with JSX, but what about those props? What are they, and how do we use them?

Props are the inputs into the component, and they cannot be changed inside the component. Conceptually, props are like the arguments that get passed into a function. In practice, all of the props passed into a component come bundled together in a single object.

```js
function Welcome(props) {
  return <h1>Welcome, Davey!</h1>;
}

<Welcome name="Goofus" />
```

You pass props into a component as attributes in the JSX. Here, we're passing a prop called `name` with a string value `"Goofus"` into the `Welcome` component. We've seen that stateless functional components receive a single `props` argument. Each of the props that are passed into the component will appear as properties of that object.

Remember that we can embed any JavaScript expression in JSX by enclosing the expression in curly braces. So instead of hardcoding `"Davey"` in the return value of our component, let's print whatever the value of that prop happens to be.

```js
function Welcome(props) {
  return <h1>Welcome, {props.name}!</h1>;
}

<Welcome name="Goofus" />
```

`props` is just an argument to that function, so it's available as a local variable. Each prop will be a property of that argument, so we can read the value of the `name` prop with `props.name`. Stick that inside curly braces, and we're in business. Let's try this for real.

```html
  <script type="text/babel">
    function Welcome(props) {
      return <h1>Welcome, {props.name}!</h1>;
    }

    ReactDOM.render(
      <Welcome name="Goofus" />,
      document.getElementById('root')
    );
  </script>
```

Let's pass in the name prop as an attribute in the JSX down here in `ReactDOM.render`, and let's use that up in the return value of our component. `name` is just a property of this `props` argument. You could name the argument something else, and it would still work, as long as you change it both places. But it's conventional to name it `props`, since that's a nice, descriptive name.

So that's how we use props in a stateless functional component.

What about in a class component? We don't have that argument in a class.

```js
class Welcome extends React.Component {
  render() {
    return <h1>Welcome, {this.props.name}!</h1>;
  }
}

<Welcome name="Goofus" />
```

There will be a `props` property on a class component, so we can use `this.props` to retrieve that object. To read the name? `this.props.name`.

```js
const element = <Welcome name="Goofus" />;
```

In this example, the value of the prop is a string. When that's the case, you just use quotes as you would an HTML attribute. But they don't have to be strings. They can be numbers, booleans, arrays, objects, functions... whatever you want.

```js
const spells = ['Expelliarmus', 'Expecto Patronum'];
const element = <Spellbook spellList={spells} />;
```

To pass in something other than a string, enclose the value in curly braces instead of quotes. You'll notice I used camelCase to name this prop. This is a best practice, because props are accessed as object properties.

```js
const notify = function() {
  alert('Hey!');
}

const element = <Welcome callback={notify} />
```

If you pass a function as a prop, be sure you don't include parentheses after the function name. That would invoke the function on the spot and pass its _return value_ as the prop. To pass the function itself, be sure to leave those parentheses off!

Props give us a way to pass data into a component. That's really handy for passing data between components. Maybe you have a piece of data that lives in the _state_ of one component, but you need to read it from a child component. You can pass it into the child component as a prop! It's really common to pass callback functions into a child component like this, as well. We haven't dealt with state or parent and child components yet, but that gives you something to look forward to.

Say, you know what the spread operator is? Looks like an ellipsis&mdash;three dots? Well, you can use spread attributes as a cool shorthand for passing multiple props into a component. Take some time to check that out on your own.

Now that we have a handle on props, we should talk about **state** next. Until then, I'm Davey Strus. Haaaappy hacking!
