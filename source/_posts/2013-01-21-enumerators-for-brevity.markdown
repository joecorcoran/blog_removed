---
layout: post
title: "Enumerators for brevity"
date: 2013-01-21 11:55
comments: true
categories: ruby code-style enumerable enumerator
---

I've recently started to reach for Ruby's `Enumerator` class more often and thought it was worth a quick mention. The functionality I'm focusing on here has been around since Ruby 1.8, but I rarely see it used in the wild.

<!--more-->

`Enumerator#each_with_index` allows us to iterate through a collection of objects, with access to the index of each object.

```ruby
numbers = [1,2,3,4,5]
numbers.each_with_index do |number, idx|
  puts "#{number} at index #{idx}"
end

# => 1 at index 0, 2 at index 1...
```

Passing the index into the block like that can be really useful, but `#each_with_index` is on its own; other common `Enumerable` methods are not afforded this kind of special extended treatment. Thankfully though, some of these methods return an `Enumerator` when they are used without a block.
```ruby
numbers.map # => #<Enumerator: [1, 2, 3, 4, 5]:map>
```

This means we can chain `Enumerator` methods like `#with_index`.

```ruby
numbers.map.with_index # => #<Enumerator: #<Enumerator: [1, 2, 3, 4, 5]:map>:with_index>
```

It's especially useful in places where you might otherwise use a placeholder array.

```ruby
cities    = ['New York City', 'Oslo']
countries = ['USA', 'Norway']

places = []
cities.each_with_index do |city, idx|
  places << "#{city}, #{countries[idx]}"
end
places # => ['New York City, USA', 'Oslo, Norway']

places = cities.map.with_index do |city, idx|
  "#{city}, #{countries[idx]}"
end
places # => ['New York City, USA', 'Oslo, Norway']
```

Check out just how many `Enumerable` methods return an `Enumerator` by [searching this page](http://www.ruby-doc.org/core-1.9.3/Enumerable.html) for "an_enumerator".

There's more reading on this topic, including a decent explanation of the external iteration features introduced in 1.9, [over at Wikibooks](http://en.wikibooks.org/wiki/Ruby_Programming/Reference/Objects/Enumerable).