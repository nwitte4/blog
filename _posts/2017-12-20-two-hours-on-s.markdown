---
layout: post
title:  "Two hours lost to the letter S"
date:   2017-12-20 12:01:31 -0500
---

I worked on a group project called [Trippin'](https://trippin-app.herokuapp.com/) which is essentially a trip planning app that lets you garner inspiration from other people's trips and events with the option to attend other people's events or recreate that event on your own. In addition to this, there is a visual component; we implemented the Google Maps API to render a map with event-markers. There are three maps: a map for all of the events created, a map for an singular trip with all the events associated with that trip, and a map for a singular event.

So, long story short, we deployed the app to heroku with just a few issues. I figured I would highlight them here for documentation purposes and maybe someday this will help someone:

On the original deployment:

My terminal informed me that my application was missing a secret key:
```
 Devise.secret_key was not set. Please add the following to your Devise initializer: (RuntimeError)
```

And then it gave me a secret key that looked like this: config.secret_key = 'lots of letters here'. Googled it. Found help. Added the secret key to my config/initializers/devise.rb file. Did my git add/commit/push to heroku master. Solved it. It deployed but the background image was gone and no maps. Googled "background image no showing up heroku". Found help. Added this to config/environments/production.rb:

```
config.assets.compile = true
config.serve_static_assets = true
```

Again, add/commit/push and *boom* background image. The ONLY thing left to do was figure out the Google Maps situation. After my group partners and I googled a bit we realized we needed to set a config var with ENV because we weren't pushing our secrets.yml up to heroku and that's where our API key lived. So we do the following:

1. Add a config var to heroku with the following terminal command:
```
$ heroku config:add GOOGLEMAPS_API_KEY='we-put-our-api-key-here'
```
Heroku confirmed this worked with some motivational terminal responses and when we click on our app in heroku > our app's name > settings > reveal config vars and sure enough, we had one called GOOGLEMAPS_API_KEY and next to it was our key.

2. Access our config var with ENV['GOOGLEMAPS_API_KEY']
We could not figure out what to do here and there was not much help online. Most people suggested setting our key equal to our new variable but this continued to give us a ["InvalidKey"](https://developers.google.com/maps/documentation/javascript/error-messages#invalid-key) error. However, before we created our GOOGLEMAPS_API_KEY variable we were getting a "NoApiKeys" error which led us to believe it was syntactical (C-L-A-S-S-I-C). So now I'm thinking, hmm, a lot of these people are working with script tags and we were working with a javascript_include_tag and it looked something like this:
```
<%= javascript_include_tag "http://maps.google.com/maps/api/js?v=3&key=VAR_NAME_HERE" %>
```
So we decide to do this:

```
<%= javascript_include_tag "http://maps.google.com/maps/api/js?v=3&key=#{ENV['GOOGLEMAPS_API_KEY']}" %>
```

MIND YOU. At this point in time, we had been opening up our heroku app and getting an HTTP error that informed us there was an issue with HTTPS so it was being run on HTTP. I didn't save the error and I'm sorry but I didn't totally understand it. More or less, the outline of the map wouldn't even show up on the page until we fixed this issue.

Basically, we removed the 's' from https:// and refreshed the page. The http error disappeared and our physical map would show up on the page with an error message within it which then led us to the API errors in the console.

Back to the story, we changed our tag to this:
```
<%= javascript_include_tag "https://maps.google.com/maps/api/js?v=3&key=#{ENV['GOOGLEMAPS_API_KEY']}" %>
```

And it worked. I realize the narrative structure here isn't entirely clear but we did the first few things in about 20 minutes maybe and spent the next two hours fiddling around with syntax with a ton of things not working only to finally come to the conclusion that it may have been the difference of an 's'. We just happened to interpolate the key at the same time that we changed the 's' and it finally fell into place. The end.


#TLDR: We spent two hours changing code all over our application before realizing we needed to change our URL from http to https.
