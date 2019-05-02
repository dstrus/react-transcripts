Hey, everyone. Welcome to Kenzie Academy. I'm Davey Strus.

Let's talk about React.

What is React? Let's turn straight to the source. Facebook developed React, and they say that it's a _declarative_, _efficient_, and _flexible_ JavaScript library for building user interfaces. It encourages the creation of _reusable UI components_ that present data that changes over time. Neat! OK, what does all that mean?

React is declarative, as opposed to imperative. With declarative code, you describe _what_ you want to do. With imperative code, you describe _how_ to do it. Let's look at a non-code example.

Here's one way to handle my arrival at a restaurant:

> Hi there. I'm Davey. This is Nicole. I'd like you to grab two menus, a couple of sets of silverware wrapped in a napkin, and escort us to that table right there. No, that one. Set a menu and a set of silverware at each seat, invite us to sit down, tell us about the specials, and alert our server that we have arrived.

What do you think? Was that declarative or imperative? Definitely imperative. I explained step-by-step _how_ I wanted it done. That's fine, if you're not into the whole brevity thing. Here's the declarative version of the same scenario:

> Table for two, please.

That's declarative.

How about driving a car with an automatic vs. manual transmission? Driving a manual is imperative. you have to tell the car _how_ to do everything. With an automatic, you just step on the gas.

How does this apply to code? Let's look at an imperative way to multiply all the items in an array, versus a more declarative way.

```js
function multiplyByThree(originalArray) {
  let multipliedArray = []

  for (let i = 0; i < originalArray.length; i++) {
    let multipliedNumber = originalArray[i] * 3
    multipliedArray.push(multipliedNumber)
  }

  return multipliedArray
}
```

OK, I've got an array. Give me a new, empty array. Now give me a counter, start at zero. Multiply the current item in the array by 3. Now push that number onto the new array. Hey, remember that counter from earlier? Add one to it and keep doing that as long as the counter is smaller than the length of the first array. Once that's no longer true, give me that second array. That's the more imperative version.

```js
function multiplyByThree(originalArray) {
  return originalArray.map(number => number * 3)
}
```

OK, I've got an array. Multiply each item in the array by three and give me the resulting array. That's more declarative.

React is declarative.

It's also efficient.

Doing stuff to the DOM is fairly slow, at least in comparison to manipulating other objects. React is designed to re-render the entire application every time the state changes. That's important, and it's different from a lot of other frameworks and libraries. It also sounds pretty slow.

So React has something called the Virtual DOM, which is a bunch of JavaScript objects that mirror the real DOM, but the page doesn't need to redraw every time you change anything. So when the state changes, React updates the virtual DOM and compares that to a snapshot to determine which parts of the real DOM actually need updating. It then changes only those portions of the real DOM that need changing. That's much more efficient.

So React is efficient.

It's also flexible.

React apps are comprised of components. In the words of the documentation, "Components let you split the UI into independent, reusable pieces, and think about each piece in isolation." This gives you a great deal of flexibility, as you'll see when we discuss components in more detail.

So that's quick introduction to React. I can't wait to show you how to build applications with it. Until then, I'm Davey Strus. Haaaappy hacking!
