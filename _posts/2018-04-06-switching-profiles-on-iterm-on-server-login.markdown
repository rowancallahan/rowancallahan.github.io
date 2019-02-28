---
layout: post
title:  "auto changing Iterm2 profile on login"
date:   2018-04-06 02:08:46 -0400
categories: jekyll update
---
Changing iterm2 profiles on server login can be hugely helpful if you want to make sure that you never make the mistake ofrunning dangerous commands on 
an environment that isn't safe for it.

After setting up the profile via shell integration and following all the instructions it turns out that I was missing something
I needed to send the "trigger" command to the iterm2 shell
the command is in this [stackoverflow][docs] article and is as follows.

{% highlight bash  %}

cd ~
vim .bashprofile
echo -e "\033]50;SetProfile=Foo\a"

{% endhighlight %}

```
success!

```

If you have installed the unix additions package on iterm2 as well and the profile that you are switching to "foo" here is the name of it you
should be good to go!

Check out the [stackoverflow][docs] for more info on this and more information 

[docs]:https://stackoverflow.com/questions/8598021/iterm-2-profiles#8600248 
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
