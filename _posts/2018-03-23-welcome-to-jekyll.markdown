---
layout: post
title:  "Changing Jekylls default home page"
date:   2018-03-23 22:08:46 -0400
categories: jekyll update
---
Jekyll has a ton of great features but one thing that was absolutely bamboozling me wwhen I first started to use it was the way to switch which markdown file would be used as the home page.
Its possible to just shuffle around pages within the server that you are hosting on after Jekyll successfuly compiles, but this ended up breaking things on the site that I didn't want to break and was much more trouble than it was worth.
After going through a whole post on the way to override themes, I realized that this was what I was looking for. Jekyll renders pages based on what the markdown file is titled. This was something that I didn't quite understand at first but after changing the name of about.md to index.md and switching my index.md to posts.md I finally go tthe effects I wanted.
Switching this and rerunning the jekyll build command now set the index.md rendering as the index.html in my sites folder and I could use this as my "homepage" now.

Now it was time to start testing out the build process

{% highlight bash  %}

cd ..
jekyll build --source sourcefolder --destination Sites

{% endhighlight %}

```
success!

```
Jekyll is an incredibly powerful tool, so powerful that I almost fully accept that there are huge parts of it I will never understand, even in the stack overflow explanations.

Changing the home page of the site however, is one of the most basic things that you can do, and something that left me scratching my head for far too long.
To all the confused people trying to set up their jekyll website on a home server, I hope this is helpful on getting everything set up and ready to go.


Check out the [Jekyll docs][jekyll-docs] for more basic info and also definitely check out their documentation on overriding theme defaults for more help with these types of basic questions. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
