Hey, everyone. Welcome to Kenzie Academy. I'm Davey Strus.

We now know a bit about what Redux is, now let's explore the different pieces in a bit more depth.

The bummer of Redux is that there are quite a few pieces to it, and they only really work together. The individual pieces aren't useful without the others.

This is somewhat in contrast to React itself. Even if you never touch state or props, you can use React as a UI library to break pages into components. You won't enjoy many of the benefits, but it _will_ work. Then you could add state to your components, but never use props to pass data between components. It would still work, and you could take advantage of the fact that state changes will automatically re-render the appropriate portions of the UI. Then, when you're comfortable with that, you can start using props to pass data between components. You don't have to use _all_ of React to enjoy the benefits of _portions_ of React.

For better or worse, this isn't really true of Redux. You can't use Redux without the **store** where your data lives. But you can't change the state of the store without a **reducer**. But the only way to make use of a reducer is to dispatch an **action**. So you need _all_ of these pieces in place before Redux really does anything useful. That can feel a little tedious when you're learning, so bear with me for a bit. In time, you'll come to understand that centralizing state with Redux saves us from a whole lot of other, equally tedious tasks, and gives us greater confidence in the state of our application along the way. So let's dig into all of these pieces and see how they fit together.

First, there's the **store**. This is that single source of truth from the first principle. The API for the store is pretty small.

You create the store with `createStore()`, passing it a reducer—one of those pure functions from the third principle—and, optionally, the initial state.

You access the state from the store with `store.getState()`. You initiate changes to state by dispatching actions with `store.dispatch()`—because the second principle tells us you can't update it directly. And you listen for changes to state with `store.subscribe()`.

As you may have noticed, I couldn't even tell you the basics about the store without referring to reducers and dispatching actions. So we really need to understand what those things are before we can wrap our heads around the store itself. All right. Let's talk about actions.

**Actions** are simple JavaScript objects that describe something that can happen in our app—like adding a new record, or updating an existing one. They are the _only_ source of information for the store, and we send them to the store using `store.dispatch()`.

```js
{
  type: 'ADD_NOTE'
}
```

Suppose we're making a note-taking app. Here's an action we might have. It's just an object, and there's only one property that's absolutely required: `type`. By convention, the value of `type` is an all-caps, snake case string. To help save us from typos, we typically define the type as a string constant, because we'll refer to it in more than one place, and our editors can help us catch typos if we save it to a constant first.

```js
const ADD_NOTE = 'ADD_NOTE'

{
  type: ADD_NOTE
}
```

This obviously doesn't save you any typing—you're writing the same set of characters, just without the quotation marks—but if you mistype it, your editor will complain that you're trying to use an identifier that hasn't been defined. Or at the very least you'll get an `Uncaught ReferenceError` at runtime. If you just hardcode the string literal everywhere, it will probably just silently fail, and it will be much harder to track down the problem.

Although `type` is the only required property, most of the time you'll have some sort of data payload alongside it.

```js
const ADD_NOTE = 'ADD_NOTE'

{
  type: ADD_NOTE,
  body: 'I <3 Redux.'
}
```

After all, the action contains _all_ of the information that you're going to send to the store about what is to take place. So any information it needs to actually make the change has to be there. The structure of this data, however, is entirely up to you, as long as there's a `type`.

```js
const ADD_NOTE = 'ADD_NOTE'

{
  type: ADD_NOTE,
  title: 'Deep thoughts',
  body: 'I <3 Redux.'
}
```

Maybe notes have a title as well as a body.

Suppose notes need a unique identifier.

```js
const ADD_NOTE = 'ADD_NOTE'

{
  type: ADD_NOTE,
  id: 44,
  title: 'Deep thoughts',
  body: 'I <3 Redux.'
}
```

You'd better include that identifier in your action, because the actual state change is determined by a reducer, which, as a pure function, can't do things like generate unique identifiers, because then it could return different output given the same input. The point is, any information that's needed to actually make the change must be part of the action.

Suppose you're deleting a note.

```js
const DELETE_NOTE = 'DELETE_NOTE'

{
  type: DELETE_NOTE,
  id: 44
}
```

Then the ID is probably the only information you need in your action aside from the `type` itself.

So that, briefly, is what an **action** is. We send these actions to the store with `store.dispatch()`.

```js
store.dispatch({
  type: ADD_NOTE,
  id: 44,
  body: 'I <3 Redux.'
})
```

You can pass an action directly to `dispatch()` like this, but most of the time, you'll write some functions called **action creators** that _return_ actions.

```js
function addNote(id, body) {
  return {
    type: ADD_NOTE,
    id,
    body
  }
}

store.dispatch(addNote(44, 'I <3 Redux'))
```

Here I'm using an action creator called `addNote()`. There's nothing special about action creators. They're just functions that return objects. But they're reusable and portable and save us from hardcoding object literals all over the place, every time we want to dispatch an action, so again, we can take advantage of the features of our code editors.

If you want to dispatch an `ADD_NOTE` action by hard-coding the action object, you'll have to remember exactly what the payload in that object is supposed to look like. To figure that out, you might actually have to read through the code of a reducer function every single time to see what properties it uses. That's a pain in the neck, and it's incredibly error-prone.

If you use an action creator, then your editor may pop up the signature of that function as you type, so you can see exactly what arguments it expects.

You can go a step further and create **bound action creators**. That might look like this...

```js
const boundAddNote = (id, body) => dispatch(addNote(id, body))
```

Instead of passing your action creator to `dispatch()` every time, you can create another function that does that. Then you can just call that _bound_ function, and it will invoke the action creator and `dispatch()`.

Again, you don't even _have_ to use action creators, much less _bound_ action creators, but in practice, you often will. At a minimum, you have to dispatch actions.

So then what happens? By dispatching an action, you've told the store what happens, but not how the application's state changes in response. For that, we need a **reducer**.

Reducers are functions that receive two arguments—the previous state, and an action—and return the new state, which may or may not be different from the previous state. The reducer has to decide how to change the state, if at all, based on that action object.

```js
function notes(state = {}, action) {

}
```

Let's see how we might write a reducer to handle an `ADD_NOTE` action. The reducer, like all reducers, will receive the previous state and the action. It's pretty common to use [default argument syntax](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/default_parameters) from ES6 to set an initial state, in the event that there _is_ no previous state, so here I'm setting it to an empty object.

Now this reducer _must_ return the new state. So let's just start by returning the previous state, unchanged.

```js
function notes(state = {}, action) {
  return state
}
```

That way, if the reducer is invoked with any actions that we don't know how to handle, or that this reducer is ultimately not responsible for, we won't mess up the existing state.

Now let's try to handle the `ADD_NOTE` action type.

```js
function notes(state = {}, action) {
  if (action.type === ADD_NOTE) {

  }
  return state
}
```

Again, I'm using the `ADD_NOTE` constant I defined earlier to check the type, so I don't hardcode a string literal, which is error-prone.

Let's assume that we keep an array of notes at `state.notes`. Always start with a copy of the portion of state that you're modifying, rather than modifying it directly. So I'll use spread syntax for that, then I'll push on the new note, getting the necessary data from the action.

```js
function notes(state = {}, action) {
  if (action.type === ADD_NOTE) {
    const notes = [...state.notes]
    notes.push({
      id: action.id,
      body: action.body
    })
  }
  return state
}
```

Then I'll return a new state object, either using `Object.assign` or object spread syntax, if we're in an environment that supports it, to be sure that we're returning a new object.

```js
function notes(state = {}, action) {
  if (action.type === ADD_NOTE) {
    const notes = [...state.notes]
    notes.push({
      id: action.id,
      body: action.body
    })

    return {
      ...state,
      notes
    }
  }
  return state
}
```

Now, you'd likely have a single reducer that handles all note-related actions, and rather than have a bunch of `if` statements or a big `if... else`, you'd use a switch. So let's rewrite it that way.

```js
function notes(state = {}, action) {
  switch(action.type) {
    case ADD_NOTE:
      const notes = [...state.notes]
      notes.push({
        id: action.id,
        body: action.body
      })

      return {
        ...state,
        notes
      }
    default:
      return state
  }
}
```

Now we have a reducer that at least handles one action type. Handling more action types is just a matter of adding more cases to the `switch` statement and writing the appropriate code.

We just need to be sure that when we create the store, we tell it about our reducer.

```js
import { createStore } from 'redux'
const store = createStore(notes)
```

Now we've covered the basics of Redux. Nothing we looked at here is specific to React. You can use all of these same pieces to manage state in a vanilla JavaScript app. If you _are_ using React, there are some helpers that you'll want to know about.

Next time, we'll actually build a little something with Redux. Until then, I'm Davey Strus. Haaaappy hacking!
