Welcome to the Kenzie Academy Woooorld of Sports! I'm Davey Strus.

It's time for Hard mode. Hard mode is all about [lifting state up](https://reactjs.org/docs/lifting-state-up.html)—moving state out of the `Team` component and into the `Game` component. Now, you may be asking, "Why?" Hard mode does not actually ask us to do anything that would _require_ moving it, but if you look ahead to the extra credit, it says to create a `Scoreboard` component that displays both teams' scores. So once we make that component, it will look something like this. I'll put the `Scoreboard` component above the venue, and the scoreboard would need both scores, wouldn't it? So at that point, if the scores were being stored in the `Team` component, you'd have no way to share it with the `Scoreboard` component. That's what _lifting state up_ is all about: Putting the state at a high enough level that it can be passed down to all the components that need to read it.

So we're going to keep in the `Game` component instead of in the individual `Team` components, and we're going to use props to pass them back down to the teams and display them there. In the process, we're going to change `Team` to a stateless functional component, because it can be one at that point. It no longer needs state, so we're going to make it stateless. And because I'm mean and wanted to create a little more work for us, we're also going to have to change `Game` from a stateless functional component to a stateful component. So we're going to practice making _both_ changes: From stateless to stateful, and from stateful to stateless. All right? We can do it!

Let's start by changing the `Game` component from a stateless functional component to a class component. So instead of `function Game(props)`, we have, `class Game extends React.Component`. The `return` statement should now be inside the `render()` method. And we know we're going to need a constructor. Our constructor will receive props. We'll pass those props along to `super()`, and we'll initialize some state.

```js
class Game extends React.Component {
  constructor(props) {
    super(props)

    this.state = {

    }
  }

  render() {
    return (
      <div className="Game">
        <div className="stats">
          <Team
            name={props.visitingTeam.name}
            logo={props.visitingTeam.logoSrc}
          />

          <div className="versus">
            <h1>VS</h1>
          </div>

          <Team
            name={props.homeTeam.name}
            logo={props.homeTeam.logoSrc}
          />
        </div>
      </div>
    )
  }
}
```

So what do we need in state? We need the stats for both teams, right? We also are going to have a "reset game" button a little later on, but we won't worry about that yet. Let's just worry about moving state out of `Team` and into `Game`. We're going to need the stats for each team, so let's put `homeTeamStats` and `visitingTeamStats` in here. And for each of them, we will have the number of shots taken and the score, both of which we'll initialize to `0`.

```js
  constructor(props) {
    super(props)

    this.state = {
      homeTeamStats: {
        shots: 0,
        score: 0
      },
      visitingTeamStats: {
        shots: 0,
        score: 0
      }
    }
  }
```

The first `<Team />` is the visiting team. Let's pass down the stats object. So let's give `Team` a `stats` prop equal to `this.state.visitingTeamStats`.

```js
<Team
  name={props.visitingTeam.name}
  logo={props.visitingTeam.logoSrc}
  stats={this.state.visitingTeamStats}
/>
```

And let's give the other one a `stats` prop set to `this.state.homeTeamStats`.

```js
<Team
  name={props.homeTeam.name}
  logo={props.homeTeam.logoSrc}
  stats={this.state.homeTeamStats}
/>
```

We're not done with this, by the way, but we'll come back to it when we actually run into trouble. Let's start looking at how we need to change our `Team` component.

Right now, it is stateful. We can change it at the end, but for now we no longer need to initialize state in the constructor. All the stuff related to audio can also go up into the other component. So let's just get rid of that, and move it to `Game`.

```js
class Game extends React.Component {
  constructor(props) {
    super(props)

    this.state = {
      homeTeamStats: {
        shots: 0,
        score: 0
      },
      visitingTeamStats: {
        shots: 0,
        score: 0
      }
    }

    this.shotSound = new Audio('./assets/audio/smb_fireball.wav')
    this.scoreSound = new Audio('./assets/audio/smb_1-up.wav')
    )
  }

  // ...
}
```

Why are we doing that? Now that the state lives in `Game`, that's where we actually have to handle the shots. When a team takes a shot, we're going to have to, we'll have to figure all of that out in this component, because only this component is allowed to change this component's state, and we have to change the shots and the score whenever we handle the clicking of the "Shoot!" button. So even though the button lives down in `Team`, we're going to handle it up in `Game`. So let's take that method, `shotHandler()`, and move it up. Put that between our contstructor and our `render()` method.

```js
  constructor(props) {
    // ...
  }

  shotHandler = () => {
    // ...
  }

  render() {
    // ...
  }
```

It's going to need to know which team did the shooting, so let's pass that in here as an argument. In order to update state, we're going to need to know which team it was.

```js
  shotHandler = (team) => {
    // ...
  }
```

Before we figure out the score, let's make a new variable called `teamStatsKey` that will hold which key in state we're modifying the stats for. So if `team` is a string that says `'home'` or `'visiting'`, then our key will be `${team}TeamStats` (`homeTeamStats` or `visitingTeamStats`). So when we call `shotHandler()`, we need to make sure that we pass in either `'home'` or `'visiting'` as a string.

```js
  shotHandler = (team) => {
    const teamStatsKey = `${team}TeamStats`
    // ...
  }
```

Then we'll still get the score. We'll play our sound. Then if our condition is truthy, we'll increase the score. And then, when we set state, this is going to be a little more complicated, isn't it? Inside our callback's return value, we'll need to hit either `homeTeamStats` or `visitingTeamStats`. Since that's variable, we'll have to put that inside brackets, `[teamStatsKey]`. That will be set to an object, which will contain `shots` and `score`.

```js
    this.setState((state, props) => ({
      [teamStatsKey] : {
        shots: state.shots + 1,
        score
      }
    }))
```

Make sense?

So when does this actually get called? It happens when we click the button, and the button is down in `Team`. Hmm. So how do we run this method in `Team`? Well, just like we can pass data down as props, we can pass a method down as props. Totally works.

So for `Team`, we'll just pass along `this.shotHandler` as `shotHandler`.

```js
<Team
  name={props.visitingTeam.name}
  logo={props.visitingTeam.logoSrc}
  stats={this.state.visitingTeamStats}
  shotHandler={this.shotHandler}
/>

// ...

<Team
  name={props.homeTeam.name}
  logo={props.homeTeam.logoSrc}
  stats={this.state.homeTeamStats}
  shotHandler={this.shotHandler}
/>
```

Although I'm giving the prop the same name as the method, I wouldn't have to. In fact, let's just change it so that it doesn't match. I'll name the method `shoot()`.

```js
  shoot = (team) => {
    // ...
  }
```

```js
<Team
  name={props.visitingTeam.name}
  logo={props.visitingTeam.logoSrc}
  stats={this.state.visitingTeamStats}
  shotHandler={this.shoot}
/>

// ...

<Team
  name={props.homeTeam.name}
  logo={props.homeTeam.logoSrc}
  stats={this.state.homeTeamStats}
  shotHandler={this.shoot}
/>
```

So to get to it from `Team`, it'll be `props.shotHandler()`. From inside the `Game` component, it is `this.shoot()`. I think that pretty much does what we need to do from the `Game` component until we start worrying about that "reset" button. Now let's see how we need to change the `Team` component.

Well, we no longer need a constructor, because it's not going to be a class anymore. Let's just get rid of that. And let's change the signature from `class Team extends React.Component` to `function Team(props)`. And then everything in the `render()` method just becomes the body of the `Team` function.

Now, everyplace I have something like `this.state` will need to change, right? `this.shots` becomes... I called the prop `stats`, right? So `this.state.shots` becomes `props.stats.shots`. Same with `score`. And `this.props.name` becomes just `props.name`, right? And just `props.logo`. And `this.shotHandler` is now `props.shotHandler`, right? But the thing is, `shotHandler` is `this.shoot` up in `Game`, and it takes `team` as an argument, so we need to pass an argument to it. We could use `bind()` to do that, or we could just turn the event handler into an arrow function. And the argument needs to be either `'home'` or `'visiting'`, so `Team` needs to know whether it's the home or visiting team. So something else we could do is instead of passing `this.shoot` as `shotHandler`, what if we passed down an arrow function? Just make it an arrow function that calls `this.shoot()`, passing in `'visiting'` or `'home'`.

```js
<Team
  name={props.visitingTeam.name}
  logo={props.visitingTeam.logoSrc}
  stats={this.state.visitingTeamStats}
  shotHandler={() => this.shoot('visiting')}
/>

// ...

<Team
  name={props.homeTeam.name}
  logo={props.homeTeam.logoSrc}
  stats={this.state.homeTeamStats}
  shotHandler={() => this.shoot('home')}
/>
```

So this arrow function is now the value of the `shotHandler` prop. Down in `Team`, `props.shotHandler` _is_ that arrow function. So when that's invoked, it should pass the argument on exactly the way we need to.

This looks pretty good. Let's see if the stats still work... they do not. I'm getting `NaN`. So something got messed up in our `shoot()` method. It is changing, but it's changing to `NaN` every time. I think I know why.

Here is our `shoot()` method. When I get the score, I get `this.state.score`, but that's not what it's called anymore, is it? It would be `this.state[teamStatsKey].score`.

```js
let score = this.state[teamStatsKey].score
```

And down where I increment `shots`, it's not `state.shots + 1`. It's `state[teamStatsKey].shots + 1`.

```js
this.setState((state, props) => ({
  [teamStatsKey] : {
    shots: state[teamStatsKey].shots + 1,
    score
  }
}))
```

Try that. Boy, they're shooting really terribly. All right, but it seems to be working. And so does Game #2. So what did we do? We lifted state up from the `Team` component, which is now a stateless functional component, to the `Game` component, which previously was a stateless functional component. Now it is a full-blown class component with a constructor and state and all that stuff. And again, this will enable us to do things like the scoreboard later. For now, it's great practice, because this is a very common thing to need to do.

But there's one more part to Hard mode, right?

> Add a Reset Game button to the Game and a counter displaying the number of resets. When the reset button is pressed, the team stats should reset, and the reset counter should increase by 1.

OK. I'm going to put my reset button... here's my `render()` method for the `Game` component... and let's put our reset button in there with "versus".

Under my `h1`, I'll put a `div`. Inside there, I'll put "Resets:", and then I need to keep track of that. So let's keep track of it in, say, `this.state.resetCount`.

```js
<div className="versus">
  <h1>VS</h1>
  <div>
    <strong>Resets:</strong> {this.state.resetCount}
  </div>
</div>
```

So we need to add that when we initialize state in the constructor. We'll add `resetCount`, and don't forget the comma.

```js
    this.state = {
      resetCount: 0,
      homeTeamStats: {
        shots: 0,
        score: 0
      },
      visitingTeamStats: {
        shots: 0,
        score: 0
      }
    }
```

So if we just save that much and have a look, now under "VS" we see "Resets: 0". So underneath "Resets: 0", let's put a button that'll say "Reset Game". It'll need some sort of `onClick`. We'll figure that out in a minute. Let's have a look at our button.

```js
<div className="versus">
  <h1>VS</h1>
  <div>
    <strong>Resets:</strong> {this.state.resetCount}
    <button>Reset Game</button>
  </div>
</div>
```

There it is. Beautiful.

So we need some sort of method for this. We'll call it `resetGame()`. I'll put it under `shoot()`. I'll use public class field syntax, just in case. And what does it do? It resets the stats and increments the reset counter. So we're going to be calling `this.setState()`. And will it be based on the old value? It will, becase the reset counter gets incremented. So we do have to use the callback.

```js
resetGame = () => {
  this.setState((state, props) => ({
    resetCount: state.resetCount + 1,
    homeTeamStats:  {
      shots: 0,
      score: 0
    },
    visitingTeamStats: {
      shots: 0,
      score: 0
    }
  }))
}
```

That should be all that `resetGame()` needs to do. OK, here's our "Reset Game" button. Let's just throw in `onClick={this.resetGame}`, no parentheses.

```js
<button onClick={this.resetGame}>Reset Game</button>
```

How does that do? Let's take some shots, rack up a score, click reset... we're back to zero, and "Resets" says `1`. Should work on our second game as well. Every time we click it, the counter goes up, and the stats for both teams go back to zero. And they continue to tally correctly after the reset. We didn't break them somehow. Excellent!

All right, I think that does it for Hard mode. The main thing we did was lift state up from the `Team` component to the `Game` component. In the process, we changed `Team` from a class component to a stateless functional component, and we changed the `Game` component from a stateless functional component to a class component. And doing so allows us to pass all of these stats down as props to any additional component that we want, such as `Scoreboard`, which we'll add in the extra credit. And wouldn't you know it? That's next time.

Until then, I'm Davey Strus. Haaaappy hacking!
