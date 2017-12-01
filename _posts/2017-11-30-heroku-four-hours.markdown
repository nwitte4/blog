---
layout: post
title:  "I just deployed this application on heroku and it took me 4 hours!"
date:   2017-11-30 17:25:31 -0500
---

Yes four hours! Why you ask?? Javascript, that's why. My favorite and most precious language did this to me and I refused to see it and I think it's partially because I didn't want to see it? I thought, it must be heroku, it must be ruby, it must be the terminal, it must be sqlite! But no. It was javascript.

To rewind, I had completely forgotten that I had tried to do a tiny javascript function on this app to make a fun animation with the buttons. However, when I did that, it didn't really work. We had a small lesson on javascript working with rails apps where our instructor told us that if we don't format the javascript properly, the whole app breaks. Lesson learned!

Basically, when trying to push my atom file to heroku, I kept getting a "rake aborted!" message while then lead to:
```
Tasks: TOP => assets:precompile
remote: (See full trace by running task with --trace)
remote: !
remote: ! Precompiling assets failed.
remote: !
remote: ! Push rejected, failed to compile Ruby app.
remote:
remote: ! Push failed
remote: Verifying deploy...
remote:
remote: !	Push rejected to secure-chamber-15308.
```

This was really fun for me and for my TA to try to figure out. I looked all over stack overflow for an answer and it was brutalizing, trying all sorts of hacky ways to figure it out, nothing working QUITE right. I installed a few random things, probably shouldn't have? But no harm, no foul. For now.

I finally decide, I'll go to lunch and come back and try again. I look at the build log, but this time, I *really* look and I think to myself, why did I ignore this line so many times:
```
rake aborted!
remote: ExecJS::RuntimeError: SyntaxError: Unexpected token name «i», expected punc «;»
remote: JS_Parse_Error.get ((execjs):3538:621)
```

To my own credit, there were over 250 lines, many of which I am not able to parse out entirely so I feel like it was sort of similar to finding a needle in a haystack. Which also is a fun coding challenge we stumbled upon the other day!

<pre><code class="javascript">
function contains(haystack, needle) {

  // does the haystack contain the needle?
  for (var i = 0; i < haystack.length; i++) {
      if (haystack[i] === needle) return true;
  }

  return false;
}
</code></pre>

I digress, anyways I found this stack overflow answer that says, hey, open up your rails console and throw this guy in there:
```
JS_PATH = "app/assets/javascripts/**/*.js";
Dir[JS_PATH].each do |file_name|
  puts "\n#{file_name}"
  puts Uglifier.compile(File.read(file_name))
end
```

So I do, and it shows me where I'm getting an error! Woo! I hop into that JS files and I remember, "oh my god, this function I made a week ago wasn't working before but it was simply breaking internally and there was no external indicator that anything was broken (nothing was breaking on the page when it ran unless I opened up the console). I deleted the piece of javascript that wasn't working and left the functioning javascript and tried, AGAIN, to push my code onto the heroku app and holy moly it worked. It was as simple as one.single.for.loop not working to send me into a 4 hour long search that was right in front of my face. And to make things funnier, it seems to really have been a single misplaced (or not placed at all) semicolon. Damn you javascript.

The great thing is, I did it myself. I have come to accept that being a developer means working in a broken state pretty much until deployment and it's pretty cool that I was able to debug it in the end by my own volition. It made me feel powerful and also.. smart? Classic javascript being a blessing in disguise. Love you long time, JS.

Again, though, lesson learned!
