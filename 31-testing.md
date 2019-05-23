Hey, everyone. Welcome to Kenzie Academy. I'm Davey Strus.

Wouldn't it be cool if we could write automated tests for our app? Setting up a testing framework is probably a big pain though, right? Not if you used Create React App! Create React App comes with Jest, which like Create React App and React itself, comes from Facebook. It's a nice little JavaScript testing framework that works nicely with React.

So our little clock already has Jest configured, and it comes with a simple test for the `App` component: `App.test.js`. Any files named with `.test.js` will automatically be included when your tests run.

We can run the tests with `npm test`.

```shell
$ npm test
```

By default, it only runs tests that are related to files that have changed since the last commit. I'll press **a** to run all tests...

And the only test we have actually fails. Once we started using Redux, this test stopped working, because it renders the `App` component without wrapping it in a `Provider` and giving it a store. We can make that work by mocking the store, but we don't need to go to all that trouble right this minute. I'll just change this to a ridiculously simple test.

```js
it('renders without crashing'), () => {
  expect(true).toBeTruthy()
}
```

And now it passes. You can also tell what this test does just by reading it. It reads a bit like English. I'm testing that "it renders without crashing", and the implementation of the test is just, "expect true to be truthy." Now, I'm not actually testing that the `App` component renders correctly. I'm just trying to make this test pass so we can get it out of our way for the moment. Maybe we can return to it later to make it pass legitimately.

Now what's something we could actually test? How about our actions? Those are pretty simple—our action creators. Let's look at those.

In my `actions` folder, I'll create a new file called `index.test.js`, and it will be responsible for testing everything in `actions/index.js`. We can go ahead and open that alongside it.

How about `startTimer()`? That's about as simple as it gets.

```js
export function startTimer() {
  return {
    type: START_TIMER
  }
}
```

We could test that.

So we're going to need access to our actual actions in here, so let's import those.

```js
import * as actions from './index'
```

And you can make individual tests with a function called `test()`. Autocomplete is really my enemy today. `test()`, and then you give it a name for your test, and the second argument after the name of your test, is a callback in which you write a set of expectations.

```js
test('two plus two equals four', () => {

})
```

You wrap something in this `expect()` function—like an expression, like `2 + 2`—and then that gives you access to a bunch of methods called **matchers**, for example: `.toBe()`. And then the argument to the matcher completes the sentence. So just stick `4` in here, `expect(2 + 2).toBe(4)`...

```js
test('two plus two equals four', () => {
  expect(2 + 2).toBe(4)
})
```

...and let's see what happens to our test suite when I run that. All right, now I have two passing tests. Excellent.

There are a whole bunch of matchers. Be sure to check out the [documentation](https://jestjs.io/docs/en/using-matchers) to learn a little more about them. There's all kinds of good stuff here. `toBe` is a common one. `toEqual` is another one. For something like a number, `toBe` and `toEqual` will work the same, but for an object they will not. `toEqual` is usually what you want when you're dealing with an object, becaue `toBe` uses `Object.is` from ES6, which tests that objects are exactly the same—the same object—not simply the same value. So `toEqual` is what you want if you just want to check that the values are equal, but for something like a number, `toBe` and `toEqual` work exactly the same way.

You saw me use another matcher earlier, `toBeTruthy`. `toBeFalsy` is another one, `toBeGreaterThan`, etc. Check out the [documentation](https://jestjs.io/docs/en/using-matchers) to see more.

We can group tests together inside little _suites_, and you can use `it()` instead of `test()`, and then make this read like a sentence.

```js
it('does addition correctly', () => {
  expect(2 + 2).toBe(4)
})
```

Works the same. A lot of times we group these together in little test suites by saying `describe()` and then describing what the whole group is testing. What I'm testing here are my actions.

```js
describe('actions', () => {

})
```

So this is going to be a group with multiple tests in it, grouped together in this `describe` block. So it's a little DSL—a little domain-specific language for testing. So then we can move this inside here...

```js
describe('actions', () => {
  it('does addition correctly', () => {
    expect(2 + 2).toBe(4)
  })
})
```

...and it still works, but it groups it together in a test suite. So right now I have two test suites and two passing tests. If I added another test to the same suite, we would see that the number of tests increased, but the number of test suites did not.

But we don't actually want to test that addition works, do we? That's silly. So let's start working on a real test. We're going to test `startTimer()` first, right? It should return an action to start the timer.

```js
describe('actions', () => {
  it('should return action to start timer', () => {

  })
})
```

The second argument there is the callback where I actually write my expectations.  So what I'll do, ultimately, is run that action creator and test its return value against what I expect it to be. So I'll make a variable here called `expectedAction`, and that'll just be an object with the type `'START_TIMER'`, because that's what I expect that action creator to return.

```js
const expectedAction = {
  type: 'START TIMER'
}
```

So then I'll say `expect()` and then inside my `expect()` here, I'll actually call the action creator, so `actions.startTimer()`. And I expect that to equal (not `toBe`, because it's an object) my `expectedAction` there.

```js
expect(actions.startTimer()).toEqual(expectedAction)
```

How's it look? Uh-oh, it failed.

```
- Expected
+ Received

  Object {
-   "type": "START TIMER",
+   "type": "START_TIMER",
  }
```

What'd I get here? Expected and received. These look awfully doggone similar to me, so why didn't this work? I did say `toEqual()`... yes, not `toBe()`. So that's good. I didn't make that mistake. Oh, I forgot the underscore!

```js
const expectedAction = {
  type: 'START_TIMER'
}
```

There we go. Sometimes when your tests fail, you wrote your tests incorrectly, rather than writing your code incorrectly. That's OK. It's still worth it to have that working test when you're done. OK, so we have one that works. How about another one?

```js
it('should return action to stop timer', () => {
  const expectedAction = {
    type: 'STOP_TIMER'
  }

  expect(actions.stopTimer()).toEqual(expectedAction)
})
```

Check it out. Notice two test suites, but three total tests, because I have three tests across two test suites. The suite for `App`, and then the test suite for `actions`. Cool.

Now, I have `updateTime()`, which is slightly more complex, because it has an argument. I'll put that at the top, because it comes first in the code, but it doesn't actually matter.

OK. So my expected action is going to be a little different here.

```js
it('should return action to update time', () => {
  const time = new Date()
  const expectedAction = {
    type: 'UPDATE_TIME',
    time
  }

  expect(actions.updateTime(time)).toEqual(expectedAction)
})
```

As long as the `time` in my `expectedAction` is the `time` I pass into my action creator, then that's the `time` that should come back as part of the action object.

Let's see... all right, it passed. Cool. We just tested our actions!

So do you get how these things are formed, and the general idea behind them? Understanding how it actually works under the hood is a whole different story, but the DSL—the actual domain-specific language here—is pretty nice, and you can kind of tell how tests work just by looking at them. So, not bad.

How about our reducers? Let's test them. So inside our `reducers` folder, I'll create a new file called `clock.test.js`. Naming it that way will automatically include it among my tests. And this time, I'm going to have to import my reducer, of course.

```js
import reducer from './clock'

describe('clock reducer', () => {

})
```

And it should do some stuff. It should return the initial state. That's one thing it should do. If there is no previous state, and the action I send in is nothing in particular—not a `type` that I'm looking for—then I expect it to return an empty object as state, right? I did write it that way, didn't I? Yes, so if `state` is undefined, it'll set it to an empty object, and it will just return that by default. Let's make sure that that happens.

```js
describe('clock reducer', () => {
  it ('should return the initial state', () => {
    expect(reducer(undefined, {})).toEqual({})
  })
})
```

I expect my reducer, when called with `undefined` as the previous state, and an empty object as the action... what do I expect the return value to be? I expect it to _equal_ an empty object. Not `toBe`, `toBe` checks that they are the exact same object, not just that they have the same value. OK, does this work? All right, great. We have three test suites now, and five total tests.

Let's check something slightly more interesting, like it should handle `UPDATE_TIME`.

```js
it('should handle UPDATE_TIME', () => {
  const time = new Date()
  expect(
    reducer({}, {
      type: 'UPDATE_TIME',
      time
    })
  ).toEqual({
    time
  })
})
```

All right. I expect reducer, when called with an empty object as the previous state, and this action here with a type of `UPDATE_TIME`, and let's make a `time` here... and this particular time, what do I expect that to return? What should my new state look like? It should be an object with a time of... that time, right? Let's try that out. It passed! Great.

By the way, something you can always do—I'm not sure if I showed this to you yet or not—is just throw `not` in front of one of these matchers, like `expect().not.toEqual()`, and it will test the opposite. And we should see that fail now... good, we saw it fail. We don't really want to test it that way, do we?

We could test the other action types in our reducer, but I don't think I need to go through all of that right now. I think you get the picture. That is how we test reducers!

We also want to be able to test our components, and for that, there's another framework called Enzyme that's a little nicer than just using Jest. So next time, we'll have a look at using Enzyme to test components. Until then, I'm Davey Strus. Haaaappy hacking!
