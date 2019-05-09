Hey, everyone. Welcome to Kenzie Academy. I'm Davey Strus.

Let's try a couple of exercises that'll make us dig into React Router. We'll explore URL parameters, then we'll use the `Switch` component to show a 404 page.

First, I want to be able to browse to `/welcome/SomeName`, and have the name that's in the URL appear as the name in the `Welcome` component. In other words, if I browse to `/welcome/Nicole`, I want to see "Welcome, Nicole!" If I browse to `/welcome/Miles`, I want to see "Welcome, Miles!"

You'll have to create a new route that renders the `Welcome` component. This time, the path should be `/welcome/:name`. The colon indicates that that part of the path is dynamic. That's how we can pass data in via the URL. The data shows up on that `match` prop that React Router passes in. The `match` object has a property called `params`, which will then have a property for any dynamic portions of your path. In this case, `props.match.params.name` will contain whatever appeared in the path at that position.

If the `Welcome` component is accessed through this new route, it should display the name that was passed in via the URL. If `Welcome` is accessed through the root route—the one we already have—it should still display whatever name is passed in as a prop, as it does now. Got it?

For the second part of the exercise, modify the app so that a 404 component displays when there is no route matching the current URL. You'll need to use React Router's `Switch` component to accomplish this.

If you need help, be sure to check out the links on this page. In the next video, I'll go over a solution to each part of this exercise, but be sure to give it a shot on your own first.

Until then, I'm Davey Strus. Haaaappy hacking!
