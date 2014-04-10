---
layout: post
title:  "Hassling .Net"
date:   2014-04-09 08:00:00
categories: open-source
---

I just spent several days trying to make .Net do something it apparently cannot do.
Basically I wanted to have several .exe assemblies, with some common code. Generally
you can do this by putting the common code in either a .dll or a .netmodule. For example,
common.dll or common.netmodule, and prog1.exe, prog2.exe, prog3.exe, etc. For deployment
you need to make common.dll available to prog1, prog2, prog3 etc by a variety of means.
For common.netmodule, the file must be in the same directory as the programs that call
that module.

Back in .Net 2.0 the link.exe command was able to combine .netmodule files into
a common assembly, but that capability is no longer supported by .Net any more.
Sadly, Microsoft's documentation on this is quite abstruse, so it took me several
days to figure out that was impossible. I even try installing the .Net 2.0 SDK,
but I could find no way to install it under Windows 8.1.

Ultimately, I really did not want to deploy more files that I had to, so I had to
find another way to do what I wanted. As much as I did not want to, the simplest
solution was to set up my Maven build to copy the common source code into each
module (subproject). As it turns out, this works quite well, but it really is
a hack. 