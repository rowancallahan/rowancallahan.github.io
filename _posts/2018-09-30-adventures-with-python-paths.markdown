---
layout: post
title:  "Adventures(headaches) with paths and python"
date:   2018-09-30 03:45:46 -0400
categories: jekyll update
---
I ended up getting an odd error this weekend which kept telling me that even though I was able to compile c code correctly I had an error with my compiler.

It turns out that I had made a mistake in my path variables, and had been prepending them instead of appending them to path.

This was necessary when I started using anaconda as my source for my system wide python, and this made sense to me when I set it, however, what I forgot was that conda has its own gcc compiler within it.


This kept giving me the maddening error that said that my compiler was not working.

```
configure: error: C compiler cannot create executables
```
Luckily as per usual there was an arch forum in which someone had a very similar issue to me.
[user forum][link1]

By resetting my path so that /usr/bin and /bin where in front of my conda path compiling and everything else once again worked.

[link1]: [https://bbs.archlinux.org/viewtopic.php?id=108615]

