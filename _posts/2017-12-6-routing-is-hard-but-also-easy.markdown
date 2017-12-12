---
layout: post
title:  "Routing is hard, but also easy?"
date:   2017-12-6 20:25:31 -0500
---

So after (almost) building my fourth fully functional project, *along with 6 or 7 other partially functional projects*, on rails, I finally feel like I have a knack for routing an application properly. And so far I would have to say it's super hard, until it's not. In the beginning I was truly following my instructor word for word. I'm a pretty fast typer:

![I'm an octopus, bitch]({{ "/assets/typingspeed.PNG" | absolute_url }})

*NBD, but also if you want to [challenge me](https://www.livechatinc.com/typing-speed-test/#/), challenge accepted.*

Anyways, I typed anything he typed, verbatim, for quite some time to ensure that my application functioned exactly the way his did whether or not that meant it was broken or working. If something broke, I took it personally. But of course, I should have, because usually if something was broken it was as simple as me typing:

name:

instead of

:name

Let me tell you, that mixed me up for at least 2 days and it threw errors left and right. The best part about that ended up being that it really improved my ability to read and work through errors. I'm almost positive that if you ask any experienced developer (or maybe any developer at all?) that the ability to work through error messages is one of the most important skills to master. I mean, it makes complete sense. If you can read through and identify the error, you almost always know what to do next to fix it.

So after a few days of rails, I started listening less and less because I wanted to see what I could do on my own. I started jumping ahead every once in a while and creating my own errors.

Disclaimer: I should probably mention that when building our first few applications on rails we purposefully worked in a 'broken state' in order to better understand what was actually going on behind the scenes. For example, we would create a model called "Authors" and seed it with some data comprised of, you guessed it, author names! Now let's say we have an author#index view which listed all of our authors and we wanted to add a link to our page to create a new author with this:

<%= link_to 'Add New Author -', new_author_path %>

And it looks great and I add an new author and try to save it but then when we actually click to save we get an error:

![Routing, man]({{ "/assets/authorsnew.PNG" | absolute_url }})

On the first day I would have thought, "what does this mean, what do I do, what is a route, what is an author, I need help". Fortunately, after a few days it simply became a matter of exposure that I learned to break down and follow almost like a map with questions:

1. Where is the error occuring?
2. What is the type of error that is occuring?

These two questions usually lead me to a few more:

1. Is it a routing error? If so, am I missing a route completely?
2. If not, is it a specific page that's breaking?
3. Is it a variable?
4. Have I misspelled that variable?
5. Have I forgotten to initialize a variable? Have I initialized it wrong?
6. Is it a method error?
7. Am I calling the method wrong or am I calling it on the wrong type of object?
8. Did I forget to end one of my if statements or loops? Classic.

I'm sure there is a plethora of other errors I'm used to getting at this point but those are the ones that I can think of off the top of my head.

I think the main point of this article is routing is extremely confusing when you start getting errors but when you learn to read errors it's actually incredibly easy to target those errors and fix them.

Maybe this article is about the importance of competence when it comes to reading errors. Go figure.
