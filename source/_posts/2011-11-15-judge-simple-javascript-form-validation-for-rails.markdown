---
layout: post
title: "Judge: Simple JavaScript form validation for Rails"
date: 2011-11-15 13:17
comments: true
categories: 
---

Since I've just hit version 1.0.0 and settled on what I think is a developer-friendly interface, I thought it was probably time I wrote a bit about Judge, my client side form validation gem for Rails 3.

<!--more-->

## Context

There are some other gems out there that attempt to solve this problem, so it would be remiss of me not to mention them. [Client Side Validations][csv] is feature-rich and does a lot of work for you.  There's also [LiveValidation][lv], which has been around for a long time and might be a good choice for people with legacy Rails apps that need Prototype.

I wanted a solution that was as small as possible. This is a simple job – no need for middleware, no need for extensions to core Ruby classes.  I also wanted it to be as flexible as possible - no assumptions should be made about form markup, styles, classes and so on. I started hacking away, using the jQuery plugin pattern as a starting point for the client side code. It soon became clear that jQuery had to go. Writing Judge as a jQuery plugin was a mistake – the code I wrote ended up tightly coupled to specific event bindings, specific DOM elements etc. That kind of stuff should really be left for the user to decide.

## Examples

### Setup

Add validation to an attribute in your model.

{% codeblock lang:ruby %}
class Thing < ActiveRecord::Base
  validates :name, :presence => true
end
{% endcodeblock %}

Use the <code>Judge::FormBuilder</code> in your view.

{% codeblock lang:ruby %}
<%= form_for(@thing, :builder => Judge::FormBuilder) %>
{% endcodeblock %}

Add <code>:validate => true</code> to any form element that you wish to validate on the client side.

{% codeblock lang:ruby %}
<%= form_for(@thing, :builder => Judge::FormBuilder) do |f| %>
  <%= f.text_field :name, :validate => true %>
<% end %>
{% endcodeblock %}

### Quick validation

Validate the <code>:name</code> input with JavaScript.

{% codeblock lang:javascript %}
judge.validate(document.getElementById('thing_name'));
  // => { valid: false, element: HTMLInputElement&hellip;, messages: { blank: "Can't be blank" } }
{% endcodeblock %}

Validate on keyup (requires jQuery).

{% codeblock lang:javascript %}
$('input#thing_name').keyup(function() {
  var input = $(this).get(0);
  console.log(judge.validate(input));
});
{% endcodeblock %}

### Validation with watchers

{% codeblock lang:javascript %}
// instantiate watcher
var nameInput   = document.getElementById('thing_name'),
    nameWatcher = new judge.Watcher(nameInput);

// some time later:
nameWatcher.validate();
  // => { valid: false, element: HTMLInputElement&hellip;, messages: { blank: "Can't be blank" } }
{% endcodeblock %}

### Validating stored elements

{% codeblock lang:javascript %}
// save all text input elements within form.thing-form against key 'foo'
var textInputs = document.querySelectorAll('form.thing-form input[type=text]');
judge.store.save('foo', textInputs);

// validate all elements stored against key 'foo'
judge.store.validate('foo');
  // => [ { valid: true, &hellip; }, { valid: false, &hellip; }, { valid: false, &hellip; } ]
{% endcodeblock %}

[csv]: https://github.com/bcardarella/client_side_validations "Client Side Validations gem by Brian Cardarella"
[lv]: https://github.com/alechill/livevalidation "LiveValidation by Alec Hill"