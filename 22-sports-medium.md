Ladies and gentlemen, welcome to the Kenzie Academy Woooorld of Sports! I'm Davey Strus.

OK, it's time for Medium mode. We have a couple of teams. We can shoot. We can score. Now let's make it a little more interesting.

First:

> Add functionality to make a sound play when a team shoots and a different sound when a team scores. The resources below should help accomplish this.

All right. Let's make this happen, shall we? I've got a couple of sounds downloaded here in my `assets/audio` folder, so let's use those.

This belongs in our `Team` component. I'm going to add a couple of properties to the component—not to the _state_, just to the component itself—with some audio files. So I'm going to say, in my constructor...

```js
this.shotSound = new Audio('./assets/audio/smb_fireball.wav')
this.scoreSound = new Audio('assets/audio/smb_1-up.wav')
```

I'll use those two sounds, and then in our `shotHandler()`, we'll play some sounds. I'll just do this at the beginning, right after we grab the score from state.

```js
shotHandler = () => {
  let score = this.state.score
  this.shotSound.play()

  // ...
}
```

What do you think? I need to refresh my page. Not sure how the mic picks that up, but it's making a lovely sound. OK, cool.

If we make it, we want to do that too. So let's try putting that inside our `if`.

```js
shotHandler = () => {
  let score = this.state.score
  this.shotSound.play()

  if (Math.random() > 0.5) {
    score += 1
    this.scoreSound.play()
  }

  // ...
}
```

Refresh. Cool. If we wanted to delay that just a tiny bit, we could, with `setTimeout()`. It doesn't sound too bad with them overlapping a little bit, but the sounds you choose may not be as brief as my fireball sound, so it could be that the sound for shooting hasn't finished by the time it plays the sound for scoring, and that may sound bad. So let's go ahead and put a `setTimeout()` around this. I'll just put a delay of `100`. Not very much. Just a little. Use whatever sounds right to you.

```js
  if (Math.random() > 0.5) {
    score += 1

    setTimeout(() => {
      this.scoreSound.play()
    }, 100)
  }
```

That sounds fantastic. All right, I've got sounds for shooting and scoring.

> Let's add an additional statistic to our teams. Let's display a Shot Percentage metric. This should be shots scored divided by shots attempted. This metric should only display if a shot has been taken. This should not be visible if the team has not attempted to make a shot.

All right, very interesting. Let's make it happen!

To make this show conditionally... let's worry about that part a little later. First, let's just make it display.

So down in our `render()` method, underneath "Score", let's put another `div`. Inside there, "Shooting %" and then... something. Let's calculate it up here in our `render()` method.

```js
render() {
  const shotPercentage = this.state.score / this.state.shots

  // ...

  <div>
    <strong>Shooting %:</strong> {shotPercentage}
  </div>

  // ...
}
```

See how that looks. OK, `NaN`, not a number. Can't divide by zero, so let's go ahead and hide it. Let's say `if (this.state.shots)`, because if that's zero, that will be falsy, right? Then we'll calculate then, and then let's make a variable containing some JSX. We can totally do that! We better declare the variable outside the `if`. Inside the `if`, we'll set `shotPercentageDiv` to a little JSX expression.

Down below, we'll just put `{shotPercentageDiv}`, which will either be `undefined` or the `div`. For `undefined`, it just won't render anything, but if we've assigned it because some shots were taken, then it'll display. It should also take care of our divide-by-zero issue.

```js
render() {
  let shotPercentageDiv

  if (this.state.shots) {
    const shotPercentage = this.state.score / this.state.shots
    shotPercentageDiv = (
      <div>
        <strong>Shooting %:</strong> {shotPercentage}
      </div>
    )
  }

  return (
    // ...

    {shotPercentageDiv}

    // ...
  )
}
```

OK, let's refresh. I don't see it. I take a shot, and there it is. Very good. Ah, you see that though. The repeating decimal looks slightly ridiculous. Plus, we're getting a decimal instead of a percent, so we should multiply that times 100, shouldn't we? All right, no problem. Let's take this calculation times 100, and let's just put all of that inside `Math.round()`.

```js
const shotPercentage = Math.round((this.state.score / this.state.shots) * 100)
```

There we go. I like it! Pretty good.

So we have shooting percentage that only displays when a shot has been taken.

Next...

> Create a Game component that accepts a venue name as a prop.

OK. I'll put this aftert `Team`, before `App`. Let's see what it says here about this. Can this be a stateless functional component, or not?

> Update your default component that is currently displaying your teams to display the Game component instead. Move everything the default component was displaying into the Game component

> Update the Game component to display "Welcome to" followed by the name of the venue passed in as a prop.

> The default App component should only render the Game component instead of rendering the Team components as before.

> The Game component should display the venue name for the game and the Team components that were previously displayed in the default App component.

> Each team should function as before but now conditionally display a Shot Percentage metric if a shot has been taken by the team.

So far at least, it sounds like it could be stateless, so let's make it functional.

```js
function Game(props) {

}
```

Cool. And it should receive a prop called `venue`. It's going to display the teams now, and `App` is only going to display the `Game`. So let's take the `.stats` from `App`...

```js
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
```

...and let's make `Game` return some JSX inside a `div` with a `className` of "Game", and we'll throw our stats in there.

```js
function Game(props) {
  return (
    <div className="Game">
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

And then in the `App` component, we'll just render `Game`, and we'll pass in a `venue`, right? "Union 525 Gem". Yes, that's G-E-M, not G-Y-M.

```js
function App(props) {
  return (
    <div className="App">
      <Game venue="Union 525 Gem" />
    </div>
  )
}
```

OK, that's my venue. I want to display that venue in the `Game` component. So, above my stats, I'll put an `h1`: `Welcome to {props.venue}`.

```js
function Game(props) {
  return (
    <div className="Game">
      <h1>Welcome to {props.venue}</h1>
      <div className="stats">
        {/* ... */}
      </div>
    </div>
  )
}
```

Did we do it right? Hey, looks good! All right!

So, to review, we created a brand new component called `Game` that accepts some props. And everything we were previously displaying in `App`, namely our two teams inside our `.stats` div, we moved to `Game`. And all `App` does is render `Game`. That's keeping `App` nice and simple. All the hard work is being done elsewhere.

This also means we could now have two games, potentially.

> Think about what would happen if we added another Game component to our App. We could change the Venue name but we can only create new games with the same teams. What could we modify in the Game and Team components to allow us to have unique teams for each game we include in our default App component?

All right, very interesting. So right now, we could stick a second `<Game />` in `App`, and we could change the venue to "Sheridan Arena" (not a real place)...

```js
function App(props) {
  return (
    <div className="App">
      <Game venue="Union 525 Gem" />
      <Game venue="Sheridan Arena" />
    </div>
  )
}
```

...and then we have "Welcome to Sheridan Arena" and then two new instances of our same teams. Each instance of the team is keeping track of its own _shots_ and _score_, however, so that's nice. We can totally have two games going, but they're between the same teams. So how can we have two games between different teams? What could we do differently? Well, I think we would have to have props on `Game` for each of the teams. Right?

So inside our `App` component, I'll make a variable for the Raccoons, and that has a `name` of "Russiaville Raccoons", and a `logoSrc` of the path to that image.

```js
function App(props) {
  const raccoons = {
    name: 'Russiaville Raccoons',
    logoSrc: './assets/images/raccoon.png'
  }

  // ...
}
```

Let's make another team—just a little object with all the data related to the team.

```js
  const squirrels = {
    name: 'Sheridan Squirrels',
    logoSrc: './assets/images/squirrel.png'
  }
```

Cool. And then I can do is have `homeTeam` and `visitingTeam` props on my `Game` component.

```js
    <div className="App">
      <Game
        venue="Union 525 Gem"
        homeTeam={squirrels}
        visitingTeam={raccoons}
      />
      <Game
        venue="Sheridan Arena"
        homeTeam={squirrels}
        visitingTeam={raccoons}
      />
    </div>
```

But I want the second game to have different teams. I did download a couple of other logos. I have `bunny.png` and `hound.png` images here.

```js
  const bunnies = {
    name: 'Burlington Bunnies',
    logoSrc: './assets/images/bunny.png'
  }

  const hounds = {
    name: 'Hammond Hounds',
    logoSrc: './assets/images/hound.png'
  }
```

And then in my second instance of `<Game />`...

```js
      <Game
        venue="Sheridan Arena"
        homeTeam={bunnies}
        visitingTeam={hounds}
      />
```

So now I'm passing information about the two teams into the `Game` component, but the `Game` component still has them hard-coded in there, doesn't it? So, let's just change that! Instead of saying `name="Russiaville Raccoons"`, I'll say, `name={props.visitingTeam.name}`, etc.

```js
function Game(props) {
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
```

Do you see why this works? If we want more than one instance of the `Game` component, each with its own teams, we're going to have to pass the team information in as _props_, which means we have to do it up here in `App`. Information about the teams needs to live up in the `App` component.

Let's try this out. And look at that! Yeah! At the Union 525 Gem, we have the Russiaville Racoons vs. the Sheridan Squirrels, and in Sheridan Arena, we have the Hammond Hounds vs. the Burlington Bunnies. Excellent. And each of the four teams is keeping track of its own statistics. Check that out.

I believe that is it for Medium mode. Now, Hard mode _is_ hard, and we'll definitely use some tricks that you haven't done before. But I encourage you to give it a shot if you haven't yet. If you've given it a shot, and you want to see my solution, then come back next time, and we'll have a look at Hard mode.

Until then, I'm Davey Strus. Haaaappy hacking!
