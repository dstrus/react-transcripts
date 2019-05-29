Hey, everyone. Welcome to Kenzie Academy. I'm Davey Strus.

Testing is a really important part of software development, and we haven't said a peep about it yet! Obviously, you've been doing testing of a sort all along: Running your code, trying things out, clicking around and seeing if the reality matches your expectations. But it hasn't been very formal, and it certainly hasn't been automated. In the next few videos, we'll look at some automated testing frameworks and write a few tests for our React app. But first, let's talk a little more about testing, in general, and how the automated tests that we write fit into the whole process.

If you someday work for a company with a real development team or teams, and a good, established process, there may be dedicated QA folks. To be clear, I mean quality assurance. Testers! So that means you, the developer, don't have to worry about tests, right? No, it doesn't mean that.

For one thing, there's more than one type of testing. There's white-box testing, where you have access to the source code, and black-box testing, where you don't. And then there are different testing methodologies, like end-to-end testing, integration testing, regression testing, unit testing—they all serve specific purposes, and they can all be important in certain scenarios. Then you'll hear terms like functional testing and acceptance testing, and it doesn't help that not every developer or every organization uses all these terms exactly the same way. It can be a bit much! Well, I'm going to explain _all_ of it _right now_, in this 15-hour video! Just kidding. Fifteen hours wouldn't be enough! But we'll see if we can get a handle on _some_ of it.

How about unit testing versus end-to-end testing? These are kind of opposite extremes, so we ought to be able to explain the difference. In unit tests, you're testing the smallest thing you can possibly test—like a single function, all on its own. You don't worry about how that function fits together with other objects—just how that function works in isolation.

On the other extreme lie end-to-end tests. These test how everything works together, how processes work end-to-end. This generally even includes testing the UI. If you have dedicated QA folks, there's a pretty high probability that they'll be doing exclusively end-to-end tests. We can still write automated end-to-end tests, but you'll always need some manual testing.

What about unit tests? Making sure that each function, each class, each component, works on its own? Who's responsible for those? That is usually the responsibility of the developer who wrote the code, and the tests we'll be writing in the next few videos are unit tests. So let's talk about them a bit more.

Sometimes, backend code is pretty heavily unit tested, while the front-end is mostly covered by end-to-end tests. But there's still value in unit testing front-end code, and we're going to learn how.

So, unit tests...

* test the smallest thing we can test
* require no external resources
* are repeatable

Let's go over what those really mean, one at a time.

Unit tests test the smallest thing we can test. Something like a single component, a single class, or even a single function. In a unit test, we don't worry about how they fit together, just that they work as individual _units_, hence the name. Whatever's being tested—the individual function, class, or component—is sometimes referred to as the **unit under test**, or UUT—the thing currently being tested. I said that when you're unit testing a function, you're just concerned with how that function works in isolation. In other words, given certain input, does it produce the expected output? You'll find that testing in general is all about whether reality matches your expectations. You expect your code to produce certain output. Does it? Then the test passes.

Can you see why pure functions are much easier to test than impure functions? With a pure function, you know that the output will always be the same, given the same input. That's really easy to test, because it's perfectly predictable. If your function's output is dependent on something else—like a piece of application state that isn't passed in as an argument, then your test is going to involve some additional setup to get the application into a known state—or to at least simulate the application being in a known state. That's doable, but it's messier, and it reminds me of another point...

Unit tests require no external resources.

Your unit tests shouldn't rely on the availability of an external API. It shouldn't really even be dependent on something like the system clock. Nothing outside the application, and ideally, nothing outside the unit under test—the specific class or function. So what if your _code_ is dependent on something external? How do you test it? Well, you can fake it. If your code is dependent on an API, you can set up your test so that calls to the API return a simulated HTTP response—something predictable. Remember, you're not testing the API. You're testing that this one unit of your application makes the correct call to the API, or perhaps that it does the right thing with what it gets back from the API. If you want to test that it handles _bad_ output from the API, simulate bad output from the API, and write a separate test for that. Want to test how your code handles the API being unavailable? Simulate that, and write a test. Get it? You need to simulate external resources, so that your tests are predictable. After all...

Unit tests are repeatable. You should be able to run them over and over, and, assuming that you didn't change your code, they should give the same results every time. This can be harder than it sounds. I guarantee that at some point you'll have a unit test that fails only part of the time. It'll drive you batty. But hey, it provides a strong incentive to think about how testable your code is as you're writing it.

So once again, unit tests...

* test the smallest thing we can test
* require no external resources
* are repeatable

So what does a unit test look like? How do we write them? One pattern that you'll often see is the AAA pattern: Arrange, Act, Assert.

**Arrange**: Initialize objects, set up the data that you'll be passing to the unit under test—any inputs to that unit.

**Act**: Actually _invoke_ the unit under test. If you're testing a function, this is where you run the function.

**Assert**: Verify that the output is what you expected.

How about a quick example?

```js
function add(a, b) {
  return a + b
}
```

Let's test this function. It's nice and pure, so it should be straightforward to test. And nevermind the particulars of how we set up a test case. Let's just assume we've done that and complete our three A's.

First: Arrange. What do we need to arrange? The function has two inputs, so let's initialize those.

```js
// Arrange
let a = 3
let b = 4
```

That should be all the setup we need.

Now: Act. Time to invoke the function.

```js
// Arrange
let a = 3
let b = 4

// Act
let result = add(a, b)
```

Finally: Assert. Exactly what this looks like will depend on the specific testing framework that you're using, and in particular what assertion library you're using. But you'll use some library for making assertions that throws an error if the assertion fails. Most of these libraries use one of three styles: Assert, should, or expect.

```js
// Arrange
let a = 3
let b = 4

// Act
let result = add(a, b)

// Assert
let expectedResult = 7
assert.equal(result, expectedResult)
```

Here's what it might look like with a library that uses the _assert_ style, such as Node's built-in [assert](https://nodejs.org/api/assert.html) module. Call `assert.equal()`, and pass it two values: The actual result, and the expected result. If the two arguments aren't equal, `assert.equal()` will throw an error, and the test will fail. If they're equal, it will pass.

Another assertion style is the _should_ style, which you'll see in Behavior-Driven Development, or BDD.

```js
// Arrange
let a = 3
let b = 4

// Act
let result = add(a, b)

// Assert
let expectedResult = 7
result.should.be.exactly(expectedResult)
```

Here's what that same test looks like in _should_ style, using, for example, [should.js](https://github.com/tj/should.js/). "Result should be exactly expected result." You can see that this style reads like English. This is essentially a DSL, or Domain-Specific Language. Even though it's really just JavaScript, it no longer _reads_ like JavaScript.

A third style, and the one we're going to use in the next few videos, is _expect_ style, where your assertions are written as _expectations_.

```js
// Arrange
let a = 3
let b = 4

// Act
let result = add(a, b)

// Assert
let expectedResult = 7
expect(result).to.be(expectedResult)
```

Here's what that might look like using, for example, [expect.js](https://github.com/Automattic/expect.js). This style also uses a DSL, so the assertion ends up reading like English: "Expect result to be expected result." Nice.

We'll use the Jest framework in the upcoming videos, and it uses _expect_ style assertions, but the syntax is a _tiny_ bit different from expect.js.

```js
// expect.js
expect(result).to.be(expectedResult)

// Jest
expect(result).toBe(expectedResult)
```

Instead of `to.be()`, it's just a method called `toBe()`.

So those are assertions, and as a general rule, you should try really hard to have only one assertion per test. If you're tempted to write more than one assertion, you probably really want a second test.

So what should you test, anyway?

You might hear folks talk about code coverage. Code coverage measures how much of our code our tests actually execute—usually a percentage of lines of code. You probably aren't going to aim for 100% coverage most of the time, but there are some things to look out for when deciding what to test. In particular, look out for **execution paths** and **boundary conditions**.

What's an execution path? It's a possible path through a function. Any time you have an `if` or a `switch`, you're creating at least one more execution path. If you're testing a function with more than one execution path, take care that you have tests that hit each one.

What about boundary conditions? These are those values that trigger a difference in behavior.

```js
function isItAParty(attendees) {
  if (attendees < 5) {
    return 'Nah, more of a gathering.'
  } else if (attendees < 15) {
    return 'Yes, a *small* party.'
  } else {
    return 'A party for sure!'
  }
}
```

Here, we have three different execution paths. We want to be sure to test each path, but we also want to be careful to test the boundary conditions, so test values like 4 and 5, 14 and 15. This helps you test, for example, that you didn't use "less than" when you really meant "less than or equal to."

Before we get on with writing real tests, another quick note: Tests are code, and like all other code, they, too, must be maintained. So take care when writing your tests. Use all the good habits you've developed, keep your tests DRY, etc.

Next time, we'll begin actually adding tests to our application. Until then, I'm Davey Strus. Haaaappy hacking!
