---
layout: post
title:  "Modern Web Apps"
date:   2014-07-03 17:00:00
categories: open-source
---

Back in the late 1990's I was experimenting with a web app based on the Dynamic HTML practices.
It started up rather conventionally as a cgi-bin script written in TCL, but the rest was rather
unconventional. Most of what was loaded into the browser was JavaScript, and much of that JavaScript
code was responsible for generating more JavaScript code, eventually rendering some HTML. The data
for the app was loaded with the JavaScript, and the final configuration was downloaded back to the
cgi-bin script with a POST request.

The application was basically a configuration tool for some software we were packaging for our
X.400 and X.500 servers. One thing that worked really well was the app was blazingly fast;
instant start-up, and far more responsive than any web app I had ever experienced before.
The downside was it was a rats nest of spagetti code, written quickly, and hard to modify and
maintain. It could have been better written, but mostly it was an experiment and a proof of
concept on a new technology.

After that I have worked on more conventional web apps using pretty standard technologies.
Some were Java based, such as applets and web-start, some used server-side rendering such as
Java EE, JSP, etc. The Java apps generally performed well, but the start-up was painfully slow.
The HTML apps suffered from both slow and unpredictable start-up, as well as operation.

A decade and a half later I am learning to use the current generation of Single Page Apps,
or SPAs, using Ember, and I get this feeling of deja vu. Aside from these new apps starting
up fast, and performing with crisp responsiveness, the principle is pretty much the same:
load up a massive amount of JavaScript that ultimately renders the final HTML on the client
side instead of the server side.

Fortunately, it is much easier to build effective web apps that do not degenerate into a
rat's nest of spagetti code:

1. [Ember](http://emberjs.com) adds Object Oriented features to JavaScript, as well as many
   other features for general programming and UI implementation.
2. [Require](http://www.requirejs.org) lets you modularize your code similar to Java import
  statements.
3. [WebJars](http://www.webjars.org) further improves modularity and collaboration via
   aritfact repositories.

Of course, no matter how wonderful the platform, APIs, or language, it is always possible
to write crappy, unreadable code; but at least with the right tools and dicipline, we can
all do a better job.
