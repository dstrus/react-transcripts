Hey, everyone. Welcome to Kenzie Academy. I'm Davey Strus.

I'm really excited to tell you about React Developer Tools. The dev tools make troubleshooting React apps much easier, and they provide insight into our components, including state and props.

You'll find a link to the devtools GitHub repo on the page for this lesson. From there, you can find the appropriate link to install React DevTools. It's available as a browser extension for Chrome or Firefox, or as a standalone app. The Chrome extension can also work in Opera, and the standalone app can work with Safari or even with React Native. I'll be using the Chrome extension.

You should consider looking through this README for more tips at some point, but I'm going to switch over to our React app now.

I'll open up the browser's developer tools by choosing "Inspect" from the right-click menu. Anytime React Dev Tools recognizes that the page you're on uses React, you'll find a new "React" tab in your browser's dev tools. I'll switch over to that tab now.

You'll see two tabs in React DevTools: Elements and Profiler. I won't be covering the "Profiler" tab in this video, but you should check it out on your own.

On the "Elements" tab, you'll see a hierarchy of React elements, with `<App>` as the root. With `App` highlighted, look below, and you'll see that it lists the component's props. The `App` component doesn't have any.

See where it says `<App> == $r`? Do you know what that means? It means that this component—this instance of the component—is available as the variable `$r` in the console. It's very similar to what you can do in the browser's "Elements" tab. You can highlight an element, and it will be available in the console as `$0`. So we have `App` available as `$r`.

Now, this is slightly deceptive. It works great with class components, but functional components don't really have instances. Nevertheless, devtools shows that `$r` there. The truth is, it may or may not work if we try it out in the console, but there's nothing we could really do with a stateless functional component in the console even if it did work. So let's try a class component that has some state.

How about `Contact`? Select that, and then look below. You'll see props, which is empty, and then you'll see state. `formData` and `submitted` are both there. Try toggling `submitted`. The UI changes between the form and the "thank you" message as we do it. Now try actually changing it in the UI by submitting the form and clicking the "reset" button. Notice that every time state changes, the dev tools highlight the specific data that changed. Now let's see what we can do in the console with `Contact` selected.

`$r` is the `Contact` component. Good. `$r.props` is an empty object, but it _is_ there. It's not undefined.

`$r.state` gives us our state object. This gives us a good opportunity to demonstrate what happens—or rather, what _doesn't_ happen—when you modify state directly. Try `$r.state.submitted = true`. Nothing happened. Flip over to the "React" tab, and you'll see that `submitted` is, in fact, true. We did update state, but the UI did not update. Let's try again using `setState()`: `$r.setState({ submitted: true })`. Now the UI updates!

Say, we could probably run the `resetForm()` method manually in the console, couldn't we? `$r.resetForm()`... yep! That works. What about `handleSubmit()`? That doesn't work, because we don't have an event to pass in. You could fake it if you really wanted to.

Let's flip back to the "React" tab now. We don't have very many components here, but it can get pretty tedious to click through this tree looking for the component you're interested in. Don't miss that there's a search box up here. Just start typing part of the component name, and it filters that tree view for you. Nice!

That's a quick overview of React Developer Tools. Play around with it, check out the preferences... knock yourself out. We'll be using the dev tools from time to time in future videos. Until next time, I'm Davey Strus. Haaaappy hacking!
