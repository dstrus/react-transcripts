Hey, everyone. Welcome to Kenzie Academy. I'm Davey Strus.

Last time, we wrote a few automated tests with Jest. We tested some action creators and a reducer. Those are just functions, right? Ordinary JavaScript functions. They happen to be playing a special role in our application, but they're very ordinary functions, and thus pretty straightforward to test.

React components are sort of a special case, even though some of them are just pure functions. They output React elements, and to properly check that output, it would be really useful to be able to traverse those elements the way we traverse the DOM, looking for certain selectors and certain content.

Jest probably isn't the best tool for that particular job. There are a few different testing frameworks designed to work great with React components. Today, we'll look at Enzyme.

Unlike Jest, Enzyme is not included in Create React App, so we have to add it. That means installing not only Enzyme itself, but an adapter for a particular version of React. At the time I'm recording this, React 16 is current, so I'll include the adapter for React 16.

```shell
npm i --save-dev enzyme enzyme-adapter-react-16
```

Notice that I'm using the `--save-dev` flag. Enzyme isn't a dependency of running our application, only of developing it. We don't need it to be in the production bundle, so we include it among our development dependencies.

Thankfully, there's no configuration necessary to use Enzyme with Jest. Our Enzyme tests will run right alongside our other tests automatically. So let's try testing our `Clock` component.

I'll make a new file in `src/components/Clock` called `Clock.test.js`. We'll need to import React. We'll need to import Enzyme itself, as well as a function for rendering the component. Enzyme gives us a few choices for that, in case we want to do _shallow_ rendering, and not completely render all the subcomponents. We'll import `mount`, which does full DOM rendering. And then we need to import the adapter. We also need the component that we're testing.

```js
import React from 'react'
import Enzyme, { mount } from 'enzyme'
import Adapter from 'enzyme-adapter-react-16'

import Clock from './Clock'
```

After our imports, we need to tell Enzyme about the adapter.

```js
Enzyme.configure({ adapter: new Adapter() })
```

Now we can start testing it. We'll put in a describe block to start a new test suite...


```js
describe('Clock component', () => {

})
```

...and in there, we'll put our first test.

```js
describe('Clock component', () => {
  it('should render self and subcomponents', () => {

  })
})
```

For every test we write for this component, there's some common setup we'll need to do. We'll need to define the props that we want to pass to the component, and we 'll need to render the component. We'll also want to have both the props and the rendered component available in our test, so we can actually test them. Let's make a setup function above our actual tests to do all that.

What props does our `Clock` component expect? It wants a `time` prop, containing a `Date` object.

```js
function setup() {
  const props = {
    time: new Date(),
  }
}
```

It also wants a function called `startTimer()`. We don't need to give it the _real_ `startTimer()`—the bound action creator. As long as it's a function, that will do for testing purposes.

```js
function setup() {
  const props => {
    time: new Date(),
    startTimer: jest.fn()
  }
}
```

We can create a _mock_ function with Jest, by calling `jest.fn()`. That makes it a function, but it also means that Jest is watching that function, capturing any calls to it so we can do things like test the number of times it gets called and that sort of thing.

The props are all we need to be able to render our component, so we'll use the `mount` function from Enzyme to render it. We'll save the output to a variable called `wrapper`. That wrapper will let us traverse the rendered React element with an API very similar to jQuery.

```js
const wrapper = mount(
  <Clock {...props} />
)
```

Remember [spread attributes](https://reactjs.org/docs/jsx-in-depth.html#spread-attributes)? We can use the spread operator to pass an entire props object.

Now our setup function has rendered the component. We need it to return not just the wrapper, but also the props, so we have access to the date object and the mock function. We'll just return both of them in an object.

```js
return {
  props,
  wrapper
}
```

Now let's implement a test. First, let's make sure we run our setup function and grab the output. I'll use destructuring assignment to save that output in two separate variables.

```js
describe('Clock component', () => {
  it('should render self and subcomponents', () => {
    const { props, wrapper } = setup()
  })
})
```

Then I need some sort of expectation. How will we know if the component and its subcomponents rendered? If we search for, and find, an `<h2>`, then it rendered. Let's test that.

Remember, the wrapper gives us a jQuery-like API for our component. Let' find the `h2` and expect the length to be 1. That is, it should find _one_ matching `h2` element. `wrapper.find()` just takes a selector as its argument, so the string `'h2'` will do.


```js
expect(wrapper.find('h2').length).toBe(1)
```

My tests are already running, so let's check out the output... and it passed! Want to prove that it's really passing? Let's change the test so that it should fail. It's always nice to see a test fail once before it passes. That gives me a little more confidence in the results.

```js
expect(wrapper.find('h2').length).toBe(2)
```

If we expect it to be `2`, then it fails. Great! I'll change it back to something that works. And we didn't actually use `props` in this test, so I don't need to assign that.

So this tests that the component renders, but let's check that the output is actually correct. I'll add a second test to my suite.

```js
it('should display the time', () => {
  const { props, wrapper } = setup()
})
```

We'll test that it displays the time. We'll do the same setup, and this time we'll actually use the props.

We want to test the actual text content in that `h2`, so we'll find it again, but this time we'll call `.text()` on the result. That's another method straight out of jQuery's API. And we'll expect that text to contain `props.time`.

```js
  it('should display the time', () => {
    const { props, wrapper } = setup()
    expect(wrapper.find('h2').text()).toContain(props.time)
  })
```

That failed, as I secretly knew it should have. Let's look at the detailed output.

```
    Expected value:  2019-05-23T12:48:44.165Z
    Received string: "It is 8:48:44 AM."
```

We told it to expect `props.time`, so it's going to look for that nasty thing that you get when you call `toString()` on a date. We actually wanted to look for `props.time.toLocaleTimeString()`, right?

```js
  it('should display the time', () => {
    const { props, wrapper } = setup()
    expect(wrapper.find('h2').text()).toContain(props.time.toLocaleTimeString())
  })
```

There we go. Now it passes!

As you might imagine, we've only scratched the surface of testing. Even once you fully understand the tools you're using, like Jest and Enzyme, there's still the matter of knowing what exactly you ought to test. That's beyond the scope of this video, but there's a lot of material out there that you can check out on your own. In any case, you've taken your first step into a larger world.

Until next time, I'm Davey Strus. Haaaappy hacking!
