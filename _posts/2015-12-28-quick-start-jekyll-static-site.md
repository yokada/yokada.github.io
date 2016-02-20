---
layout: post
title: 'jekyll: Quick Start Jekyll static-website'
tags: jekyll
---

This post is consist of 3 steps to describe how to use jekyll to create static webisite rather quickly.

## 1. Installing some gems you will need.

Create a Gemfile:

```shell
$ vi /path/to/your/static-website/Gemfile
```

And, fill up it with the following text.

```ruby
source 'https://rubygems.org'

gem 'jekyll'
```

Run bundle install.

```ruby
$ cd /path/to/your/static-website/
$ bundle install --path vendor/bundle
```

----

## 2. Create new static-site with a built-in template

```shell
$ cd /path/to/your/static-website/
$ bundle exec jekyll new yourprojectnameishere
```

----

## 3. Run built-in server in order to check your new static site

```shell
$ cd /path/to/your/static-website/
$ bundle exec jekyll serve --port 8080
```

Finally, access to http://localhost:4040/ on your web browser.

[![IMAGE ALT TEXT HERE](/imgs/jekyll1.png)](/imgs/jekyll1.png)

----

That's all.

