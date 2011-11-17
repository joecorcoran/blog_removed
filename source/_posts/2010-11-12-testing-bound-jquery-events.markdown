---
layout: post
title: "Testing bound jQuery events"
date: 2010-11-12 21:29
comments: true
categories:
  - javascript
  - jquery
  - bdd
  - testing
  - jasmine
---

I've been using <a href="http://pivotal.github.com/jasmine/">Jasmine</a>, along with velesin's <a href="https://github.com/velesin/jasmine-jquery">jasmine-jquery</a>, to test all of my JavaScript work recently.  jasmine-jquery provides some really handy matchers, but one thing that's missing is the ability to test whether an element has a bound event.

<!--more-->

I've come up with the following matcher to do just that:

{% gist 674339 toHaveEvent.js %}

Use it like so: <code>expect($('input#foo')).toHaveEvent('keyup');</code>

I'd be interested to hear of any thoughts or improvements.  Likewise, I've started work on testing for events attached using <code>$.fn.live()</code>.  These are handled a bit differently, since live events aren't actually bound to the selected element.  Instead they sit there, bound to the <code>document</code>, listening for the event to bubble up the <acronym title="Document Object Model">DOM</acronym> tree.

{% gist 674339 toHaveLive.js %}

So: <code>expect($('input#bar')).toHaveLive('focus');</code>

Again, any improvements or thoughts are welcome.

You can make these matchers available by using <a href="http://pivotal.github.com/jasmine/matchers.html" title="Jasmine matchers"><code>this.addMatchers</code></a>.
