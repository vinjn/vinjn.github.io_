---
layout: post
title: How to add comments to Github pages
---

{{ page.title }}
================

Github pages are static html files generated from text-only markdown files hosted on github.com. Since they are static, there is no way to dynamically generate content, and you can't use a normal comment system.<br>

But we do have a solution.

It's [disqus](http://disqus.com/).


What you need to do is:

1. Create an account in disqus
2. Register your site for free
3. Copy-paste two blocks of auto generated javascipt codes to

        yourname.github.io\_layouts\post.html

This step might be diffcult for js-noobs like me, refer to [my implemetation](https://github.com/vinjn/vinjn.github.io/blob/master/_layouts/post.html) if you feel dizzy. 

The two blocks starts with 

    <!-- disqus loading --> 
and 

    <!-- disqus rendering -->
