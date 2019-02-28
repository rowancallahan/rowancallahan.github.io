---
layout: post
title:  "Blastn returns subject id as a number rather than the fasta sequence id"
date:   2019-02-28 13:50:00 -0400
categories: jekyll update
---

**Overview**
After recently using blastn for annotation, I came across an error that I had not received before. Instead of my subject id being the name of the sequence that I was using the subject id was a number. Although I was able to find someone with a similar issue on [the biostars site][biostars] this didn't appear to be an "issue" that people had trouble with.

After a while of searching I found that the issue was with the way that I had created the blast database. I had specified the option to ```parse_seqids```. This specifies that the sequence ids are parsed into numerical format and made it so that I was unable to successfully parse them after I had finished.

The only unfortunate part about this is that blast is still unable to parse anything that comes after whitespace in the sequence id, so this must be converted to a different character.

Happy Blasting!

---
[biostars]: https://www.biostars.org/p/155573/ 
