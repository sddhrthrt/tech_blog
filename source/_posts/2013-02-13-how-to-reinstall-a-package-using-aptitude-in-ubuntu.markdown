---
layout: post
title: "How to reinstall a package using aptitude in Ubuntu?"
date: 2013-02-13 13:14
comments: true
categories: howto ubuntu aptitude
---

So I was installing OpenCV - compiling it from source, infact. It needs `libgobject2.0-0.so`
which it couldn't find - I don't know why. And this problem was hardly faced by anyone
else which means it has to be because it's a pretty common library and I'd lost
it due to my usual fiddling around.

**TLDR;** The package had to be reinstalled.
<!-- more -->


How? Haha. Sounds easy? Can't uninstall and reinstall - a package manager doesn't
allow you to uninstall a package if it is depended upon by other packages. So 
what way is there to reinstall? 

RTFM.

` sudo apt-get install libglib2 --reinstall`. 

That's it.
