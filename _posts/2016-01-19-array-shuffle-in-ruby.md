---
layout: post
title: "Array Shuffle in Ruby"
tags: ruby
---

```ruby
#
# array_shuffle.rb
#
a=(1..10).to_a
(a.count).times do |i|
  r = rand(0..i)
  a[r], a[i] = a[i], a[r]
end
puts a.inspect
```

-----

```shell
╰─➤  ruby array_shuffle.rb
[9, 3, 6, 1, 5, 7, 8, 10, 2, 4]
```
