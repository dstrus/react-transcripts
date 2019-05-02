Hey, everyone. Welcome to Kenzie Academy. I'm Davey Strus.

Before we get into any particular framework or library, let's explore a brief history of single-page apps, or SPAs.

We need a few things to make single-page apps work. First, we need to be able to get new data without reloading the entire page. Then we need to be able to render that data to the page. We also need to be able to setup binding between our data and the elements of the page. And finally, we need routing to simulate the multi-page experience.

## The Dark Times

In the beginning, not only were there no single-page apps, there was no asynchronous loading of data at all. We had none of the things we needed to make single-page apps. It was all synchronous, all the time. So what _could_ you do? You pretty much had two mechanisms for adding interactivity.

Good old-fashioned hyperlinks: Click a link to send a new request and load a whole new page...

Or forms: Submit a form to... well, send a new request and load a whole new page. If the new stuff you needed to load depended on some data the user submitted, forms were your only option. Submit the form, which makes a request, the data gets handled on the server side, and you get a whole new page back.

## The Still Pretty Dark Times

Before too terribly long, JavaScript arrived on the scene. There was still no asynchronous loading of data over the network. But at least you could use JavaScript to hide and show content that you'd already loaded without making another request. That made some experiences quite a bit nicer, but it was still pretty limited.

We still didn't have any of this:

* data without reloading
* rendering data
* data binding
* routing

## XHR

With Outlook Web Access for Exchange Server 2000, Microsoft introduced a really cool new feature: XMLHttpRequest, or XHR. This allowed developers to make new HTTP requests—to get new _data_—without reloading the entire page. _Asynchronous_ requests, at last.

A few years later, Jesse James Garrett of Adaptive Path wrote a paper describing a model for developing applications with XHR. He gave it a catchy name...

Ajax! Asynchonous JavaScript and XML.

At last, we could load new data without reloading the entire page. As the X in Ajax and the X in XHR suggest, the idea was to load XML, but there was nothing about the implementation that _required_ you to use XML. And XML is a real pain to transform into HTML, so that made the next step, rendering data, a pretty nasty job.

## JSON

Thankfully, JSON came along, giving us a data format that was much easier to manipulate in JavaScript. After all, JSON is based on (and stands for) JavaScript Object Notation.

Rendering the data we loaded into a pretty web page was a solved problem. Not that developers were satisfied. We still had to manually make those Ajax calls, manually pull the elements from the page that we wanted to change, change their contents manually, etc.

## 2010

Around 2010, we started to get new libraries and frameworks that implemented data-binding. Use some sort of template in your page, and then when you change the data, the page updates automatically. Much better! So we could load new data, and redraw portions of the page based on that new data. Single-page apps, here we come! There's just one thing missing.

You see, browsers have these things called _back buttons_, and they're really handy. With single-page apps, you never actually leave that first page, but it often _looks_ like you do. Without routing, the back button would just take you clear out of the app, which feels like completely the wrong behavior.

Browsers also have these things called _bookmarks_, or _favorites_. People like to be able to go straight to a particular page on a web site. But in SPAs, there's only one page, so how do we do that?

## Routing

We do it with routing. When it feels like we've navigated to a new page, even though we technically haven't, we add a new entry to the browser's history. That fixes the back button. And when our single page first loads, we look at the URL and make sure that we render the appropriate content. That fixes bookmarks and favorites

At last, we have single-page apps that actually deliver a good experience.

So that's single-page apps in a nutshell. See you again soon! Until then, I'm Davey Strus. Haaaappy hacking!
