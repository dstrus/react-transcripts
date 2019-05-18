Hey, everyone. Welcome to Kenzie Academy. I'm Davey Strus.

Let's talk about Redux. What is Redux? Let's go straight to the source.

Redux.js.org says...

> Redux is a predictable state container for JavaScript apps.

Notice that it says, "for JavaScript apps," not "for React apps." Redux works great with React—it's become something of a _de facto_ standard for state management in non-trivial React apps—but there's nothing keeping you from using it with any other library or framework—or even with vanilla JavaScript.

Back to the official site...

> [Redux] helps you write applications that behave consistently, run in different environments, and are easy to test.

So far, it's mentioned that it's a _predictable_ state container, and that it helps you write applications that _behave consistently_. Are you noticing a pattern?

When we have bugs in our apps, it's often because our application's _state_ got out of whack somehow. When the state is being changed all over the place in all kinds of different ways, it shouldn't be that surprising that we mess up sometimes.

To quote another page on the Redux site...

> Redux attempts to make state mutations predictable by imposing certain restrictions on how and when updates can happen.

That's what it's all about, folks: Making state mutations predictable.

The intro on the web site continues...

> On top of that, it provides a great developer experience, such as live code editing combined with a time traveling debugger.

It's true. The Redux developer tools are outstanding. Because of the restrictions that Redux imposes on how state changes happen, they're able to provide tools that make debugging easier than ever.

Are you convinced yet? Ready to learn more about Redux? All right.

So Redux makes state management _predictable_. How? Let's look at the three fundamental principles of Redux.

1. Single source of truth.

With Redux, all of your state is centralized in a single store. No more keeping a little bit of state here, a little state there. It's all in one place. That makes data serialization easier and debugging faster.

Without Redux, you could keep all of your state in a single root component. It's just a pain, because you end up passing down props through untold layers of components, including passing lots of callbacks as props, and it's just... yuck.

The second principle...

2. State is read-only.

In React, we don't modify state directly, right? We use `this.setState()`... at least we're supposed to. But there's nothing actually stopping us from modifying it directly, whether intentionally or by accident. With Redux, it's impossible.

The only way to change state with Redux is to emit an **action**. We'll talk much more about actions later, but they're plain JavaScript objects describing the change you intend to make. This is part of why Redux enables such powerful dev tools. Since actions are just these plain old objects, they can very easily be stored, logged, undone, and replayed.

Third principle...

3. Changes are made with pure functions.

Actions describe a change you intend to make. Pure functions called **reducers** specify _how_ those actions impact state.

Do you remember what pure functions are?

> In a pure function, the return value is determined entirely by its inputs, without observable side effects. (https://www.sitepoint.com/functional-programming-pure-functions/)

What does it mean for the return value to be determined entirely by its inputs?

It means that pure functions always return the same result given the same arguments. This is a big part of predictability. These **reducers** receive two arguments: The current state, and an action. Given the same beginning state and the same action, state will change in exactly the same way every time, because reducers are pure functions. That means the reducer can't depend on anything other than its inputs. It can't be dependent on any other aspect of the application's state—only what gets passed to it as an argument.

Pure functions are also free of side effects, so don't go trying to modify any variables outside your reducer's scope.

Remember, reducers are pure functions that figure out how state will change. But they're functions that we write, so it's up to us to make sure they really are pure functions. Redux isn't going to enforce that. As long as we keep our reducers pure, however, state mutations will remain predictable.

So those are the three principles. We'll keep those in mind as we actually learn how to use Redux.

Redux is inspired by certain patterns and existing libraries. Chief among them is the [Flux](https://facebook.github.io/flux/) architecture. Flux uses a unidirectional data flow, and so does Redux. Redux isn't a strict implementation of the Flux pattern, but it was inspired, in part, by Flux.

So Redux gives us a structured way to access and modify a single data store, providing us with a single source of truth for our application's state. It can take a little time to understand all the moving parts of Redux in the context of a React application, but hang in there.

We need to learn about a few new concepts:

* The store
* Actions and action creators
* Dispatching actions
* Reducers

We'll start digging into these topics next time. Until then, I'm Davey Strus. Haaaappy hacking!
