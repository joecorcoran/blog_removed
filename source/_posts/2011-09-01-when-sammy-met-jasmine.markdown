---
layout: post
title: "When Sammy Met Jasmine"
date: 2010-09-01 21:37
comments: true
categories: [javascript, sammy, testing, bdd, jasmine, jquery]
---

## Testing Sammy JavaScript apps with Pivotal Labs' Jasmine BDD framework

Aaron Quint's route-driven JavaScript framework <a href="http://github.com/quirkey/sammy" title="Sammy framework on GitHub">Sammy</a>, ships with <a href="http://github.com/jquery/qunit/" title="QUnit jQuery testing framework on GitHub">QUnit</a> tests. This is an obvious choice, since QUnit is jQuery's test framework and Sammy depends on jQuery. I, however, am not the biggest fan of QUnit. It works really well, but I find the syntax a little unwieldy.  On a recent project, I thought I'd try out <a href="http://github.com/pivotal/jasmine">Jasmine – a library-independent JavaScript <acronym title="Behaviour Driven Development">BDD</acronym> framework</a> with a more readable syntax.

In this post I'll run through some typical Sammy scenarios and show you how I've written the corresponding Jasmine tests.  For reference, I'm using Jasmine 0.11.1 and Sammy 0.5.4.

<!--more-->

## Setting up Sammy

Let's start with a fairly simple Sammy app with some common features.

{% gist 560735 app.js %}

At the top of the app, the <code>context</code> variable is assigned, providing easy and unambiguous access to the <code>Sammy.Application</code> instance itself, for use elsewhere.  A GET route is declared, which simply triggers an event named <code>myEvent</code>.  The second argument to <code>trigger</code> passes an object to the event:

{% gist 560735 app_pt1.js %}

Here is the event that was triggered in the GET route.  Bound events accept a custom object as their second argument, which is used in this example to pass in some arbitrary values:

{% gist 560735 app_pt2.js %}

Here, a (very) rough example of a modular design pattern is established for any non-event driven parts of the app.  I like to group functions by similarity of purpose.

{% gist 560735 app_pt3.js %}

### An aside: Can I read all this shit easily?

I've started to ask myself this question frequently.  If the answer is no, it's time to abstract.

Particularly where jQuery is involved, I often find code that looks similar to this:

{% gist 563595 chain_soup.js %}

It only takes a few lines of jQuery, chained to the hilt, to achieve a lot.  This is seen as an advantage, and rightly so for small pieces of UI scripting, but it's a huge pain when testing larger applications since it leads to bloated specs.  Imagine trying to test the above example – a long list of calls needs a long list of assertions.  But testing needn't be a chore!  Whichever way a JavaScript project is structured, from fully modular and enclosed to fly-open-for-everyone-to-see, keeping functions succinct and limited in purpose makes for much easier testing.  Readable code == readable tests == clear statement of intentions to leave for future developers == painless development.

## Testing with Jasmine

Here's an initial attempt at testing the above Sammy app using Jasmine.  The specs are within <code>describe</code> blocks.  These aren't strictly necessary but nesting them helps to describe your tests in relation to the structure of your app.  See how the nesting follows the namespacing of the <code>url.build</code> function, for example:

{% gist 560735 appSpecWithout.js %}

Since the app involves <acronym title="Document Object Model">DOM</acronym> insertion, it's clear that some kind of setup and teardown of temporary DOM elements is required.  Using Jasmine's <code>beforeEach</code> method, the relevant elements are inserted into the DOM before each spec is run. Since each spec within this <code>describe</code> block refers to the same event, the event trigger is here as well, to avoid repetition.  The DOM elements are cleaned up using <code>afterEach</code>.

{% gist 560735 appSpecWithout_wrap.js %}

Specs are enclosed in the <code>it</code> blocks and assertions/expectations are made using <code>expect</code>.  The syntax, when read out loud, sounds pretty close to how I'd describe my tests.  "It _should etc._"; "Expect _actual_ to match _expression_".  <a href="http://pivotal.github.com/jasmine/jsdoc/symbols/jasmine.Matchers.html">Jasmine has various matcher methods</a> such as <code>toMatch</code>.

{% gist 560735 appSpecWithout_spec1.js %}

### Befriending the DOM

There's something a little clunky about certain aspects of the above tests.  In particular the DOM-related areas look a little hacky.  It would be unfair to call this a fault of the testing framework – DOM manipulation isn't even on the Jasmine agenda.  So this is where <a href="http://github.com/velesin/jasmine-jquery">velesin's jasmine-jquery extensions</a> step in.

jasmine-jquery allows for various improvements on the original tests:

{% gist 560735 appSpec.js %}

Firstly, the manual setup and teardown is no longer required, thanks to <code>loadFixtures</code>, which loads in the necessary DOM elements from an external file, inserts them into a wrapper div inside the document body, and cleans up automatically after each spec is run.  Big win.  People who are really smart might even try to load fixtures in from the same files as partials to avoid duplication (I have a feeling this could all come together beautifully with <a href="http://code.quirkey.com/sammy/docs/api.html#Sammy.Meld">Sammy's new Meld feature</a>…).  An alternative is to use <code>setFixtures</code>, which accepts either an HTML string or a jQuery element as its only argument rather than loading from an external file.

{% gist 560735 appSpec_fixture.js %}

jasmine-jquery also provides a set of <a href="http://github.com/velesin/jasmine-jquery#readme">custom matchers</a>.  Here, for example, there's no need for the workaround of checking the innerHtml of an element against a regular expression, when <code>toHaveText</code> does the trick.

{% gist 560735 appSpec_matchers.js %}

## Conclusions

The Jasmine documentation could really do with a spruce up – especially in the less immediately obvious areas like spying, mocking and stubbing.  It's possible to bundle through and get some pretty nifty async tests running (blog post on this topic pending) but clearer instructions would be a big help.  I'm sure better docs are in the pipeline with version 1.0.0 proper due to be released soon.

I find testing with Jasmine a more pleasant experience than any I've had with another JavaScript testing framework.  The only thing that has come close for me is <a href="http://visionmedia.github.com/jspec/" title="JSpec testing framework on GitHub">JSpec</a>, which has very similar syntax to Jasmine if you eschew the custom Rubyesque grammar.

Provided you structure your Sammy application in a sensible way, it's easily testable with Jasmine.  I'd like a way of testing the Sammy routes themselves, something akin to functional testing as apposed to unit testing.  But I'm happy enough with relying on the Sammy internal tests and keeping the routes as simple trigger calls.