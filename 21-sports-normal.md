Ladies and gentlemen, welcome to the Kenzie Academy Woooorld of Sports! I'm Davey Strus.

I'm not going to talk like that the whole time. Don't worry.

So hey there. How ya doin'? Let's get ready to build our sports game lab. Started out with some starter code here. Got an HTML file and a JavaScript file. This is just your basic HTML 5 document. I've prewritten some CSS, which I've linked in here. You don't have to use it, but if your markup happens to end up looking like mine, then this CSS will work for you. And we've included React, ReactDOM, and Babel from CDNs. Then here's the `script` tag for our JavaScript.

Now previously, we've put all our JavaScript inline—in between the opening and closing `script` tags. This time, we're loading it from a separate file. No big deal, except we're using Babel this time, and the way the browser version of Babel works, it loads it in via Ajax, and that's not going to work unless we run this from a web server. I have the HTML file open in my browser over here, and if I open up the console, I see that `Access to XMLHttpRequest... has been blocked`. I need to actually run a web server, so that's a little different.

So you can install a simple web server with `npm i -g serve`.

```shell
$ npm i -g serve
```

Once that's done, if you're in the folder where your project is,  just type `serve -s .`...

```shell
$ serve -s .
```

...and that will serve the app at `localhost:5000`. So open up `localhost:5000` in your browser, and there we go: "Welcome to the sports game starter." The HTML only has this `#root` element that we replace, so here is my Babel code.

```js
function App(props) {
  return (
    <div className="App">
      <h1>Welcome to the sports game starter.</h1>
      This file represents the code after completing the setup step in the lab instructions.
    </div>
  )
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
)
```

I have an `App` component, and I have my `ReactDOM.render()` call, which renders nothing but the `App` component. Looking at the setup instructions in the lab:

* Setup an HTML page that will present our application to our users.
* Setup a default component for our app that `ReactDOM.render()` renders to our HTML page. This can be a stateless component.
  * `App` is a common name used for this default component. Many components can make up our app, but we render this default component to display the entire application.
  * `ReactDOM.render()` should only ever render this component.
  * All other components will render through this component, either referenced directly in this component or as children of components referenced directly in this component.

So that's where we already are before we start writing anything else.

This will be the first time that we use multiple components in an application, so that's a new trick that we'll be learning. There are some other new tricks that we may need to learn along the way. Mostly, this will just flex the muscles that we've already been building through what we've done with React so far.

So let's start building our game. Normal mode: The `Team` component.

> Usually, in a sports game, we want our favorite team to win. In order to play this game, you're going to need to create teams so they can compete for the win.

All right.

> Create a stateful Team component that accepts a name and a path to a logo for the team as a prop.

So we'll put our new component above our `App` component. It'll need to be a class, right—because it's stateful. We'll call it `Team`. It extends `React.Component`.

```js
class Team extends React.Component {

}
```

Very good. All stateful components—all class components—must have a `render()` method. And the way I always do it, I always return some JSX: A `div` with a `className` that's the same as the name of my component. So `className` equals capital-T `Team`.

```js
class Team extends React.Component {
  render() {
    return (
      <div className="Team">

      </div>
    )
  }
}
```

All right. So I bet we're going to render this component inside our `App` component. So let me just put "Team" here, so it'll render something.

```js
return (
  <div className="Team">
    Team
  </div>
)
```

And inside `App`, I don't need these things anymore. I'll just put `<Team />` just to make sure that my `Team` component does, in fact, work. So to use one component inside another, you just put a JSX tag. That's all there is to it.

```js
function App(props) {
  return (
    <div className="App">
      <Team />
    </div>
  )
}
```

Does this work? There we go. The word "Team" showed up! So `ReactDOM.render()` is rendering the `App` component, and inside the `App` component, I'm rendering the `Team` component, which so far just has the word "Team" in it. No big deal.

All right, it'll accept a **name** and a path to a **logo** for the team as props. The component should display the team name and the logo provided through props.

All right, so we need to pass in a **name**. Our name will be the Russiaville Raccoons.

```js
<Team name="Russiaville Raccoons" />
```

Why is that pronounced "ROOSH-a-vill"? Well, you'll have to ask the founders of Russiaville, Indiana. I happen to know the answer, but I won't bore you with Indiana history right now.

So the Russiaville Raccoons—no, that's not a real team. We're trying to avoid any trademark infringement here. And we need a path to a logo. I have taken the liberty of putting a few in here, so I have an `assets` folder with an `images` subfolder, and `raccoon.png` inside that. You can find whatever logo you want. So, `./assets/images/raccoon.png`.

```js
<Team name="Russiaville Raccoons" logo="./assets/images/raccoon.png" />
```

This is wrapping, so I'll go ahead and put each prop on a line by itself, just to keep it nice and readable.

```js
<Team
  name="Russiaville Raccoons"
  logo="./assets/images/raccoon.png"
/>
```

Now inside the `Team` component, I need to actually use those props. So what can we do? How about an `h2` that displays the name? So that would be `this.props.name`, right?

```js
<div className="Team">
  <h2>{this.props.name}</h2>
</div>
```

Does that work? There we go, "Russiaville Raccoons". Excellent!

Then we want to display the logo. I'm going to put the logo in a `div` with a class of `identity`. That's how I wrote my CSS. And I'll just put an image. Don't forget, this is a self-closing tag, because images are empty, but because we're using well-formed XML syntax, we need to close everything. So `src=""`, and then let's just get the logo path out of our props. So instead of quotes there, I want curly braces. `this.props.logo`, right? And all images should have an `alt` attribute, so the `alt` attribute will be `this.props.name`

```js
<div className="Team">
  <h2>{this.props.name}</h2>

  <div className="identity">
    <img src={this.props.logo} alt={this.props.name} />
  </div>
</div>
```

Let's see how that is. Excellent.

> This component should display the team name and a logo provided through props.

Done!

* The Team component should also display some stats about the team.
  * It should display the number of Shots Taken.
  * It should display the Score for the team.
* The Team component should have a Shoot button.

We don't score every time we shoot, so we're going to give ourselves some sort of random chance of making it.

We need some stats. These stats belong, probably, in **state**, don't they? All right. We're going to need some state. We'll put our initial state inside a constructor. The constructor will receive `props`, and as always, we pass those props up to `super()` to make sure that all the functionality from `React.Component` is in place. Then we can set our initial state. `this.state` equals something. And what do we need on this state? The number of shots taken, and the score. All right. So `shots`, that'll start at zero. And `score`, that will also start at zero.

```js
class Team extends React.Component {
  constructor(props) {
    super(props)

    this.state = {
      shots: 0,
      score: 0
    }
  }

  render() {
    // ...
  }
}
```

Very good. And we need to display those things. Underneath our identity, I'll put a `div`, and I'll put "Shots:" in bold, and then `{this.state.shots}`. And then another `div`, and then in bold, "Score", and then `{this.state.score}`.

```js
class Team extends React.Component {
  constructor(props) {
    // ...
  }

  render() {
    //...

    <div>
      <strong>Shots:</strong> {this.state.shots}
    </div>

    <div>
      <strong>Score:</strong> {this.state.score}
    </div>
  }
```

OK. "Shots: 0. Score: 0". Done!

Now the `Team` component should have a "Shoot" button.

> We don't score every time we shoot, so let's discuss the shoot button's functionality.

Well, before we figure that out, let's add the button to the page, so we can at least see it. So that'll just be a button, and it'll say, "Shoot!"

```js
<button>Shoot!</button>
```

Beautiful!

* When a shot is taken, the Shots Taken count should always increase.
* There should be a random chance that the Score counter increases.

And it notes here:

> Make sure to render and test your Team component as you add new features.

We've been doing that.

The button has to do something, so it's going to need an `onClick`, and we'll write a method for that to run. Let's call it `shotHandler`. So I'll add a method called `shotHandler()`. I will use public class field syntax, so I don't have to worry about the binding of `this`. What if we just `console.log` "shoot!" when this happens?

```js
shotHandler = () => {
  console.log('shoot!')
}
```

Then down here on our button, we put `onClick={this.shotHandler}`.

```js
<button onClick={this.shotHandler}>Shoot!</button>
```

We'll worry about how it actually works here in a minute.

Let's open up our console, make sure we're refreshed, click "Shoot!" All right, it says, "shoot!" Good. It works.

But what do we actually want to do? We want "shots taken" to increase every time, don't we? So let's do that. Instead of `console.log`, what are we going to do? We going to call `this.setState()`, and because our new state is based on the previous state, we're going to need to use the callback, aren't we? So that callback receives `state` and `props`, and it returns the new state object. `shots` should be `state.shots + 1`. Remember, that's not `this.state`. It's the `state` that got passed into the callback, because this happens asynchronously, and things can get goofed up if we don't do it this way.

```js
shotHandler = () => {
  this.setState((state, props) => ({
    shots: state.shots + 1
  }))
}
```

All right. Does that work? Don't need our console anymore. Refresh the page. Yeah, "shots" went up. Every time I click, "shots" increases. Very good. Now we want a random chance that we actually make the shot, and our score goes up.

Now my raccoon is playing basketball, but we're going to worry about 2-pointers and 3-pointers and all that stuff. We're just going to score one at a time.

So, a random chance. You know how `Math.random()` works? How does `Math.random()` work? It gives us a number between zero and one, right? So there's roughly a 50/50 chance that it's going to be above or below 0.5. So we can just base our success on that. How does that sound?

So inside `shotHandler()`, we'll say `if (Math.random() > 0.5)`, then our score should go up. So how should we do this? There are several ways you could tackle this, but I think what I'd like to do is get the current score out of state, and then add to it here, and then set it down here in `setState()`.

So let's start with the current value from state. We'll say, `let score = this.state.score`. And then if `Math.random()` is greater than 0.5, I'll say `score += 1`. Then down in `setState()`, I can just say `score`, right? That's the same as saying, `score: score`. Just `score`, the shorthand in ES6.

```js
shotHandler = () => {
  let score = this.state.score

  if (Math.random() > 0.5) {
    score += 1
  }

  this.setState((state, props) => ({
    shots: state.shots + 1,
    score
  }))
}
```

And that ought to do it, so roughly half the time it should allow us to score. I shoot once, "shots" went up, "score" did not. I shoot again, "shots" went up, "score" did too! Do this a few more times. I shot 10 times, I made it 4 times. Great! This seems to work.

So when a shot's taken, the "shots taken" counter should always increase, and there should be a random chance that the score counter increases. Excellent.

So next, we set up our battle of the sports teams.

> Now that we have a default component that represents our application and we have created our `Team` component, we can start assembling the game.

* Update the default App component you created to display two teams, the home team and the visiting team.
* Use your knowledge of HTML and CSS to make the page look like more like a game where two teams, home and visiting, are facing off in competition.
* Verify that both teams are displayed and keeping track of their stats.

All right. So back in our `App` component, I am rendering one team. We should render two teams. So I'll also render the Sheridan Squirrels. How about that? I've got a squirrel PNG here.

```js
function App(props) {
  return (
    <div className="App">
      <Team
        name="Russiaville Raccoons"
        logo="./assets/images/raccoon.png"
      />
      <Team
        name="Sheridan Squirrels"
        logo="./assets/images/squirrel.png"
      />
    </div>
  )
}
```

OK, there they are, one above the other. Shoot a few times here. Shot 11 times, made it 5 times. And our lucky Squirrels made 6. Cool.

So what else could we do to make this look a little fancier? How about putting a `div` in between the two here. Actually, let's surround the two `Team` components with a `div` with the class name "stats". And then in between the two, I'll put a `div` with a class of "versus", and I'll just put a great big `h1` with "VS" here. See what that gets us.

```js
function App(props) {
  return (
    <div className="App">
      <div className="stats">
        <Team
          name="Russiaville Raccoons"
          logo="./assets/images/raccoon.png"
        />

        <div className="versus">
          <h1>VS</h1>
        </div>

        <Team
          name="Sheridan Squirrels"
          logo="./assets/images/squirrel.png"
        />
      </div>
    </div>
  )
}
```

There we go. Ooh, look at that. Yeah, "Russiaville Raccoons VS Sheridan Squirrels". Very dramatic. Shoot a few times. Four to three, Racoons. Go Raccoons! All right, cool.

So that, I do believe, takes care of Normal mode. Again, you can check out my CSS if you want to see how I made it look the way I made it look. I pulled in some web fonts to make my "VS" look like this, and to make these look the way they do. And other than that, it's just some pretty basic layout stuff. I use some `box-shadow` here, some `text-shadow` here to make things look fancy. But nothing too crazy.

All right, so that's Normal mode. Next time, we'll take a look at Medium mode. Until then, I'm Davey Strus. Haaaappy hacking!
