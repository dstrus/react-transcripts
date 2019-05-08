Hey, everyone. Welcome to Kenzie Academy. I'm Davey Strus.

You can't get too far making web apps without dealing with forms. We know all about handling forms with the regular old DOM, but in React it's a bit different, because form controls inherently have _state_. There's data tied to them, right? And that data gets updated based on user input. So these controls, like `input`, `textarea`, and `select`, are maintaining their own state.

Generally speaking, if we're using React, we want to put React in charge of our state, not the DOM. We do this by using **controlled components**. There is such a thing as an **uncontrolled component** for those occasions when using controlled components just feels too tedious, but in general, controlled components are the recommended approach to handling form data in React.

In this video, we're going to add a form to the app we've been working on, and we'll use controlled components to deal with the data.

In short, you use controlled components by setting a field's value to some value from state, so that React's state becomes the single source of truth for that component's data, and then you use event handlers to update state on subsequent user input. It'll make more sense when you see it in action.

We'll add a form to our app that accepts some contact info. So let's add a new component called `Contact`. That means a new folder inside our `components` folder called `contact`, and a new file in that folder called capital-C `Contact.js`.

This component will keep track of the form data in state, so it's a stateful component. That means it must be a class component. So I'll do what I always do when I create a new class component.

First, I import `React` and `{ Component }` from the `react` package.

```js
import React, { Component } from 'react';
```

Then I create a class called Contact that extends `Component`.

```js
class Contact extends Component {

}
```

 Now, before I forget, I make this class the default export—`export default Contact`.

 ```js
 export default Contact;
 ```

 Then I add a `render()` method that returns some JSX. I always start out returning a `div` with a class name that matches the component name, and I put the component name inside that `div` as text, just so that something shows up on the page when I first put this component in place.

 ```js
class Contact extends Component {
  render() {
    return (
      <div className="Contact">
        Contact
      </div>
    );
  }
}
 ```

Now, before I go any further with this new component, I'm going to add it to the `App` component to make sure that I have it hooked up right. There are two parts to that: importing it, and using it in the JSX.

First, I'll import it.

```js
import Contact from './components/contact/Contact';
```

Then I'll add it to the JSX just below my other components. It does not receive any props.

```js
<Contact />
```

Save it, and the word "Contact" appears on the page, so it worked. Now we can get on with this whole _controlled component_ business.

Back in `Contact.js`, I'll add a form to the JSX. I'll include _first name_ and _last name_ fields, and a submit button.

First, the form itself. Then I'll put each of the two fields inside a `div`. We'll give each one a label.

For the sake of both accessibility and usability, let's make sure we associate the labels with the inputs. There are two ways to do this. The input could be a child of the label—that is, put the input before the closing `</label>` tag. Or we can use the `for` attribute, and give it the same value as the input's `name`. Here we have another little difference between HTML and JSX. In JSX, the prop is not called `for`, like the HTML attribute. It's called `htmlFor`, camelCased.

The first field will be "First name". We'll give the input a type of `text` and the name `firstName`, and we'll make sure the `htmlFor` prop on the label matches that name.

```js
render() {
  return (
    <div className="Contact">
      <form>
        <div>
          <label htmlFor="">First Name</label>
          <input type="text" name="firstName" />
        </div>
      </form>
    </div>
  );
}
```

Then we'll do the same thing with "Last name".

```js
<div>
  <label htmlFor="lastName">Last Name</label>
  <input type="text" name="lastName" />
</div>
```

Under that, a submit button.

```js
<button>Submit Form</button>
```

You can put `type="submit"` on it if you want, but you don't have to, because `submit` is the default type for buttons that appear inside a form element.

At the moment, these are _uncontrolled_. The inputs are in charge of maintaining their own state, just as they would be if we weren't using React at all. I can type in the fields, and the appearance of those fields updates accordingly, showing whatever I type.

But we want to put React in charge of all such state, so let's initialize state. We do that in the constructor.

Let's add a constructor, remembering to receive `props` and to pass those props along to the constructor from the base class.

```js
constructor(props) {
  super(props);
}
```

Now we'll initialize state. What do we want in state? Let's keep track of whether the form has been submitted. I'll add a property to state called `submitted`, and I'll initialize it to `false`.

```js
this.state = {
  submitted: false,
};
```

Then we want our form data in state. I'll add a property to state called `formData`, and I'll make it an object, containing both `firstName` and `lastName`, both of which I'll initialize as empty strings.

```js
this.state = {
  submitted: false,
  formData: {
    firstName: '',
    lastName: ''
  }
};
```

Now that I have a place in state to store the form's data, it's time to actually turn these into controlled components. Just set the value of each input to the appropriate value from state.

```js
<input type="text" name="firstName" value={this.state.formData.firstName} />
...
<input type="text" name="lastName" value={this.state.formData.lastName} />
```

At this point, those input tags are getting really long. Rather than let them wrap, I like to put them on multiple lines, with one prop per line. I put the closing angle bracket—which has a slash in this case, since it's a self-closing tag—on a line by itself, lined up with the beginning of the tag. This is a pretty common way to format these things in React. It makes it a whole lot easier to read than just having incredibly long lines, and—spoiler alert!—they're going to get even longer in just a minute.

Do the same thing to `lastName` that we just did with `firstName`.

So now that we've set the values to something from state, they are _controlled_. React is 100% responsible for the appearance of these inputs. Let's try typing in them.

Huh. Nothing happens. That's right. The inputs are no longer responsible for their own appearance. The value in the inputs will be whatever is in state, and state is never getting updated, is it? Both of those values in state are initialized to empty strings, and then they're never being updated. What was it I said earlier?

"You use controlled components by setting a field's value to some value from state, so that React's state becomes the single source of truth for that component's data"—OK, we've done that part—"and then you use event handlers to update state on subsequent user input."

So every time the user types anything in one of these inputs, we need to update state. Only then will the inputs appear updated. So let's add an event handler. We can use a single event handler to deal with both inputs—one event handler to rule them all. I'll call it `handleChange`, and I'll use _public class field_ syntax, so we don't get into any trouble with the value of `this`, and the method will receive an event as an argument.

```js
handleChange = (event) => {

}
```

So what should this event handler _do_? It should update state, based on whatever the user typed in the form. How do we know what that is? Well, we still have an event, and that event still has a target. And that target has a name and a value.

If the user types something in `firstName`, then `event.target.name` will be `firstName`, and `event.target.value` will be whatever they typed. So we can use those two pieces of information to update state.

The key we're updating in state is `formData`, so let's start off with a copy of `this.state.formData` in a local variable. I'm a very fancy person, so I'm going to use object spread syntax to copy it. This is part of the standard as of ECMAScript 2018.

```js
handleChange = (event) => {
  const formData = {...this.state.formData};
}
```

Why do I make a copy? Because otherwise I'm changing state directly, which I generally want to avoid.

So how can I update just the part of `formData` that actually changed? How do we know which part changed? `event.target.name` should tell us, shouldn't it? So how about this?

```js
formData[event.target.name]
```

That accesses the property that actually changed. And we can set it to what? `event.target.value`!


```js
formData[event.target.name] = event.target.value;
```

Get it? I named those keys in state indentically to how I named the inputs, because I was planning ahead for this.

Now I can just call `this.setState()`.

```js
this.setState({ formData });
```

`formData` is the key we want to update, and the new value is in a variable called `formData`, so we can use that shorthand. This is the same as typing `formData: formData`.

Now we have an event handler, but we haven't actually hooked it up to the inputs. Let's do that now.

What event should we listen for? We should listen for a `change` event, so we'll use `onChange`. `onChange={this.handleChange}`, no parentheses, and it's exactly the same for both inputs.

```js
<input
  type="text"
  name="firstName"
  value={this.state.formData.firstName}
  onChange={this.handleChange}
/>

...

<input
  type="text"
  name="lastName"
  value={this.state.formData.lastName}
  onChange={this.handleChange}
/>
```

If we save, we'll find that the inputs actually show what we type now. That may seem like an awful lot of work just to get back to behavior that the DOM gives us for free, but now we also have the form data in our application's state at all times, so we can do other stuff with it. Let's just print it out underneath the form.

We'll put a `div` under the form, with `{this.state.formData.firstName}`, and let's put a line break in between the two, then `{this.state.formData.lastName}`. How's that?

```js
<div>
  {this.state.formData.firstName}
  <br />
  {this.state.formData.lastName}
</div>
```

Save, and try it out. It updates as we type. Not bad.

What should we do when the form is submitted? Let's hide the form, and show a "thank you" message.

We have a place in state for keeping track of whether the form has been submitted. We're not currently updating that, but we do have a place for it. How can we update that? It shouldn't be too hard with another event handler.

This one will handle a form submission, so I'll name it `handleSubmit`. Once again, I use _public class field_ syntax to avoid any binding issues. It again receives an event argument. We'll want to prevent default this time, just as we would with a form submission outside of React: `event.preventDefault()`.

```js
handleChange = (event) => {
  event.preventDefault();
}
```

Now, what should it do? It should set state, setting `submitted` to `true`.

```js
handleChange = (event) => {
  event.preventDefault();

  this.setState({
    submitted: true
  });
}
```

Now wire it up to the form with an `onSubmit`: `onSubmit={this.handleChange}`.

```js
<form onSubmit={this.handleSubmit}>
```

Very good. Now we want to hide the form and show a "thank you" message whenever `this.state.submitted` is true. It's **conditional rendering** time! A simple `if` ought to do the trick here. If that value is true, we'll return one bit of JSX; otherwise, we'll return some different JSX.

If `this.state.submitted`, then what should we return? I'll still return a `div` with a class name matching the name of the component. And inside there, I'll just put a paragraph with a "thank you" message: `Thank you, {this.state.formData.firstName}, for submitting the form.`

```js
if (this.state.submitted) {
  return (
    <div className="Contact">
      <p>Thank you, {this.state.formData.firstName}, for submitting the form.</p>
    </div>
  )
}
```

Otherwise, we'll just use the return statement that's already there. There's no need to put an `else` in here, because if the function hasn't already returned, we know the condition wasn't true.

Let's try it out. Type a name in there, submit... nope, looks like I put `onSubmit={this.handleChange}` instead of `onSubmit={this.handleSubmit}`. Fix that, now try it, and it works.

Now how about a "reset" button to get the form back? Yeah, let's do that.

We'll need a method to reset the form. I'll call it `resetForm`, and you know the drill: public class field syntax, the event as an argument.

```js
resetForm = (event) => {

}
```

Now what will it do? Let's make it set the state to, well, the initial state. We want `submitted` to be `false` again, and we want both `firstName` and `lastName` in `formData` to be set to empty strings.

```js
resetForm = (event) => {
  this.setState({
    submitted: false,
    formData: {
      firstName: '',
      lastName: ''
    }
  });
}
```

Now let's hook this event handler up to a button. I'll just put a button underneath my "thank you" message.

```js
<button onClick={this.resetForm}>Reset Form</button>
```

Let's try it out. Type something in the form, submit it. There's the "thank you" message. Click "Reset Form", and we get the form back, and we can do it all again. Excellent!

So it requires a little bit of setup to get controlled elements going, but once you've done that, React is back in charge of all of your data, and you can take advantage of everything that React gives you for managing your UI.

Next, we'll talk about **routing**. That's one of those fundamental features of single page apps that we talked about a long time ago. Let's finally learn how to do it.

Until then, I'm Davey Strus. Haaaappy hacking!
