---
layout: post
title: "Practical uses for Sammy"
date: 2009-07-02 21:52
comments: true
categories: [sammy, javascript, jquery, cycle, accordion, rest]
---

I've recently been looking into a new Javascript framework named <a href="http://code.quirkey.com/sammy/" title="Sammy homepage at Quirkey.com">Sammy</a>, written by the most excellent <a href="http://www.quirkey.com/blog/" title="Aaron Quint's blog">Aaron Quint</a>.  It was built on top of <a href="http://jquery.com/" title="jQuery site">jQuery</a> and intended to allow developers to build one-page applications, utilising the <acronym title="Uniform Resource Locator">URL</acronym> hash (the portion of the <acronym title="Uniform Resource Locator">URL</acronym> from the # symbol onwards) usually reserved for on-page anchors to give Javascript functions lovely descriptive routes.

I'm looking forward to writing applications in this way (and I'm trying to think of an excuse to do so), but what Sammy also kindly allows is direct, linkable access to the inner workings of some of the more usual Javascript implementations too.

I've put a few examples together to demonstrate.

<!--more-->

## jQuery Accordion

Pretty much every developer out there who deals with the front-end will have come across collapsing accordion-style lists.  They're in wide use nowadays as they make pages with lots of text look a lot friendlier (for example <acronym title="Frequently Asked Questions">FAQ</acronym> pages).  Also, people like seeing stuff move around.

But what if you wanted to retain the accordion effect, whilst providing users/clients with direct links to the page with a particular list item expanded?

<a href="http://playground.joecorcoran.co.uk/sammy/accordion/" title="Sammy / Accordion example">http://playground.joecorcoran.co.uk/sammy/accordion/</a>

Click around and notice that the <acronym title="Uniform Resource Locator">URL</acronym> in the address bar is changing. If you copy one of the new <acronym title="Uniform Resource Locator">URL</acronym>s, like

<a href="http://playground.joecorcoran.co.uk/sammy/accordion/#/section/3" title="Sammy / Accordion example link straight to expanded section">http://playground.joecorcoran.co.uk/sammy/accordion/#/section/3</a>

into a new window, you'll be taken to the same page with the 3rd section expanded.

<a href="http://gist.github.com/128728" title="Sammy Accordion code Gist">Here's my Sammy / Accordion code</a>, to demonstrate how simple it is to implement.

## jQuery Cycle

<a href="http://playground.joecorcoran.co.uk/sammy/cycle/" title="Sammy / Cycle example">http://playground.joecorcoran.co.uk/sammy/cycle/</a>

Another way in which Javascript is used quite frequently is to present slideshows. Again, we could be using Sammy to provide direct access to a slide that would usually be buried in the cycle:

<a href="http://playground.joecorcoran.co.uk/sammy/cycle/#/slide/3" title="Sammy / Cycle example part 2">http://playground.joecorcoran.co.uk/sammy/cycle/#/slide/3</a>

This one was slightly harder to implement, but still achievable with a bit of hacking around. I couldn't get the 'previous' and 'next' options from the Cycle plugin working nicely with Sammy without screwing the transitions up, but I'm sure it's possible with more work.

<a href="http://gist.github.com/130003" title="Sammy Cycle slideshow code Gist">Here's the Sammy slideshow code</a>.

## Friendly form wizard

This one is less common, but it's where my imagination took me after the first two.  It's a single page, Sammy app-powered form wizard.

<a href="http://playground.joecorcoran.co.uk/sammy/form/" title="Sammy / form wizard example">http://playground.joecorcoran.co.uk/sammy/form/</a>

The stages of the wizard are simply fieldsets hidden/shown appropriately.  There's a simple validation attempt at the end which checks for empty fields and gives helpful links back to any stage that needs to be amended.  Once all fields are filled, the submit button appears and Sammy handles the post request.  Obviously, performing more complicated validation on page would become unwieldy, but handling it with <acronym title="Asynchronous Javascript and XML">AJAX</acronym> is an option.

<a href="http://gist.github.com/134843" title="Sammy form wizard code Gist">Here's my Sammy form code</a>.

I'm really impressed with Sammy and it's definitely changed the way I think about working with Javascript for the better.  I'd certainly recommend downloading <a href="http://github.com/quirkey/sammy/tree/master" title="Sammy on GitHub">the latest version</a> (0.2.0 at time of writing) and weaving it into some of your existing Javascript – it's a nice way to familiarise yourself with Sammy whilst adding an extra slice of accessibility to your work. 

## Update, 4 Jan 2011

Just thought I'd better say this, since this post is still getting a lot of traffic.  While the examples I wrote here are quite useful, Sammy itself has changed quite a lot in the meantime.  Please make sure you check the <a href="http://code.quirkey.com/sammy/docs/index.html">Sammy docs</a> before you give yourself a headache.