Hey, everyone. Welcome to Kenzie Academy. I'm Davey Strus.

This is just a quick bonus video, because I said we might come back and actually test the `App` component properly, instead of just saying `expect(true).toBeTruthy()`. That's not much of a test, is it?

So now that we've learned about Enzyme, which we can use to do component testing, let's use Enzyme to test the `App` component. I'm going to copy a lot of the stuff from the top of `Clock.test.js`. I'm going to copy all of the imports, configuration, and setup, and I'll just copy it all into `App.test.js`, getting rid of all this nonsense that's in `App.test.js` right now.

Now this is obviously going to be a little different. We want to import `App` from `'./App'`, not `Clock` from `'./Clock'`. And `App` doesn't actually need any props, so nevermind that. And we're rendering the `<App />` component, not the `<Clock />` component. And we only need to return the wrapper, not the props and the wrapper.

```js
import React from 'react'
import Enzyme, { shallow } from 'enzyme'
import Adapter from 'enzyme-adapter-react-16'

import App from './App'

Enzyme.configure({ adapter: new Adapter() })

function setup() {
  const wrapper = shallow(
    <App />
  )

  return wrapper
}
```

So now, our actual test. `describe()`, what are we testing here? `'App component'`.

```js
describe('App component', () => {

})
```

And what can we test about this? It renders a `ClockContainer`. Let's test that.

```js
describe('App component', () => {
  it('renders a ClockContainer', () => {

  })
})
```

Sounds good. So we'll probably need to import `ClockContainer`.

```js
import ClockContainer from './containers/ClockContainer'
```

OK. Let's make sure we call `setup()` and grab our wrapper.

```js
const wrapper = setup()
```

And what can we expect?

```js
expect(wrapper.find(ClockContainer).length).toBe(1)
```

So look for a `ClockContainer` inside our rendered `App` component, and expect the length of that to be `1`.

What does our test output look like right now?

```js
Test Suites: 4 passed, 4 total
Tests:       8 passed, 8 total
```

Let's save this... oops, got one failed. OK. Scroll up—this is a long error.

```
Error: Uncaught [Invariant Violation: Could not find "store" in th
e context of "Connect(Clock)". Either wrap the root component in a <Prov
ider>, or pass a custom React context provider to <Provider> and the cor
responding React context consumer to Connect(Clock) in connect options.]
```

So the problem is, `ClockContainer` needs Redux in order to work right, and we're not wrapping `<App />` inside `<Provider></Provider>`, are we? We're not doing any of the things that make the store available to that `ClockContainer` component.

So what we can do is, instead of using `mount` out of Enzyme, we can use `shallow`, which does _shallow_ rendering, which doesn't totally render all of the child components. So you can test `App` as a unit without accidentally testing the functionality of its child components. So we'll use `shallow` instead of `mount`, and see what happens.

```js
import Enzyme, { shallow } from 'enzyme'
// ...
function setup() {
  const wrapper = shallow(
    <App />
  )

  return wrapper
}
```

Pull my test output back up, and let's save my changes... and look at that. It passes! So now we have a much better test of our `App` component. We're just testing `App` as a unit. We're not trying to test that all of the components fit together right. That's what integration tests are for, or end-to-end tests. This is just a unit test of the `App` component, and now we've successfully tested that the `App` component does, in fact, render.

All right, that's it for now. Until next time, I'm Davey Strus. Haaaappy hacking!
