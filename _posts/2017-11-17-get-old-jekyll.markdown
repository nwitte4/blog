---
layout: post
title:  "So, this is the start of my blog.."
date:   2017-11-14 17:25:31 -0500
---

This is my first post. And for my first post, my first PSA:

This blog is for me. It can also be for you! But mostly, it's for me. Maybe in the long-run it will be more for you, but this first couple of posts are going to serve as both a way for me to keep tabs on myself and a way to organize those tabs in one location: i.e. right here! Exciting, for me, and maybe for you!

My main point is that the first couple of posts might have a bit of a learning curve because right now I mostly want to just throw some of my thoughts into a 'tangible' space. On that note, I am literally learning how to work with jekyll and whilst I do this, I'll probably write some stuff about it here.

First thing I found, this is a cool way to render code-like visuals. Seems absolutely fantastic for the future-me, who will hopefully throw some code-like demos or details onto the page. It looks like this:

{% highlight ruby %}
def this_is(language)
  puts `So, I guess this is #{language}!`
end
this_is('ruby')
#=> prints 'So, I guess this is ruby'.
{% endhighlight %}

However, this is not the case with javascript as I quickly found out when I tried to do this:

{% highlight ruby %}
function thisIs(language){
  console.log(`So, I guess this is ${language}!`)
}
thisIs('javascript')
// console logs, 'So, I guess this is Javascript'.
{% endhighlight %}

I mean.. it's not the absolute worst thing I've ever seen but it's certainly not the best thing I've ever seen. In a previous [project](https://github.com/nwitte4/groupsinatrasite) I used [prism](http://prismjs.com/) which was great. However, it seems I still have a bit to learn about using plugins because I'm having trouble installing it here. Maybe I'll ask for help tomorrow during class.*


\* I'm currently enrolled in a web development intensive course at New York Code and Design Academy. It's going great and it's the first educational experience that I truly wish was longer. I'll probably get more into it later. Thanks for reading my first post.

\* Update: I figured out how to highlight javascript and I've done so in my next post using highlight.js. I had to remove jekyll from my computer and then download an older version so that I was able to customize my default.html layout page. *Wooooo*! The funny this is that after I set up highlight.js, my above javascript was highlighted properly. So in order to make this page look "right", meaning incorrect highlighting, I had to hard code it improperly. Hilarious.


{% include head.html %}
