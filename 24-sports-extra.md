Welcome to the Kenzie Academy Woooorld of Sports! I'm Davey Strus.

It's time for extra credit. The good is that the extra credit is actually quite easy now that we've done Hard mode. Had we not done Hard mode, it wouldn't be possible. We have to lift state up to the `Game` component before we can add a scoreboard. But once we have, adding a scoreboard is actually quite easy.

Let's go ahead and add it right above the `Game` component, and it can be stateless.

```js
function ScoreBoard(props) {
  return (
    <div className="ScoreBoard"></div>
  )
}
```

It's just going to show the scores for both teams. So the way I want this to look... let's put each team's score inside a `div` with the class `teamStats`. How does that sound?

Inside the first `teamStats` `div`, put an `h3` that says "VISITORS". Underneath that, the actual score—also as an `h3`. And we'll have to get these as props from the `Game` component, so `props.visitingTeamStats.score`.

```js
function ScoreBoard(props) {
  return (
    <div className="ScoreBoard">
      <div className="teamStats">
        <h3>VISITORS</h3>
        <h3>{props.visitingTeamStats.score}</h3>
      </div>
    </div>
  )
}
```

And then, in between the two, I'll just put an `h3` that says "SCOREBOARD".

```js
function ScoreBoard(props) {
  return (
    <div className="ScoreBoard">
      <div className="teamStats">
        <h3>VISITORS</h3>
        <h3>{props.visitingTeamStats.score}</h3>
      </div>

      <h3>SCOREBOARD</h3>
    </div>
  )
}
```

Then I'll put another `teamStats` `div`, but for the home team.

```js
function ScoreBoard(props) {
  return (
    <div className="ScoreBoard">
      <div className="teamStats">
        <h3>VISITORS</h3>
        <h3>{props.visitingTeamStats.score}</h3>
      </div>

      <h3>SCOREBOARD</h3>

      <div className="teamStats">
        <h3>HOME</h3>
        <h3>{props.homeTeamStats.score}</h3>
      </div>
    </div>
  )
}
```

And that's about it for the `ScoreBoard` component. Pretty simple. Now we just need to render it from the `Game` component. So I'll put that right above `Welcome to {this.props.venue}`. We'll need a prop called `visitingTeamStats`, and that will be `this.state.visitingTeamStats`. And we'll need `homeTeamStats`, and that will be `this.state.homeTeamStats`.

```js
<div className="Game">
  <ScoreBoard
    visitingTeamStats={this.state.visitingTeamStats}
    homeTeamStats={this.state.homeTeamStats}
  />
  <h1>Welcome to {this.props.venue}</h1>
  {/* ... */}
</div>
```

And it seems like that's about it. Let's save and refresh. All right, there it is. It says "SCOREBOARD", and I have `0` and `0`. Let's start playing. Look at that! Beautiful! And in our second game down here, we get the same thing. Cool. It keeps track of them separately. Click "Reset Game", and of course it resets, because resetting that in state up in `Game` re-renders the `Game` component, which sends the props back down to `ScoreBoard`, and so we get the updated stats. So everything works! This is looking good.

So again, the extra credit, in and of itself, is not particularly difficult, but it was impossible until we lifted state up out of the `Team` components, because we had to pass the stats for both teams down to `ScoreBoard`. If those were living down in the `Team` component, there'd be no way to do that. You have to lift it up to the first common ancestor of both `Team` and `ScoreBoard`, because both `Team` and `ScoreBoard` need that data. The closest common ancestor of both of them is `Game`. So it needs to be at least as high up as `Game`. All right?

And that is the lab. Don't sweat it if you couldn't figure out Hard mode by yourself. It's called "Hard mode" for a reason. But hopefully, you learned something going over my solution.

Until next time, I'm Davey Strus. Haaaappy hacking!
