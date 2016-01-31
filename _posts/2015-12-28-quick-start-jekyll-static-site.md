---
layout: post
title: 'jekyll: Quick Start Jekyll static-website'
tags: jekyll
---

This post is consist of 3 steps to describe how to use jekyll to create static webisite rather quickly.

## 1. Installing some gems you will need.

Create a Gemfile:

{% highlight sh linenos %}
$ vi /path/to/your/static-website/Gemfile
{% endhighlight %}

And, fill up it with the following text.
{% highlight ruby linenos %}
source 'https://rubygems.org'

gem 'jekyll'

{% endhighlight %}

Run bundle install.

{% highlight ruby linenos %}
$ cd /path/to/your/static-website/
$ bundle install --path vendor/bundle
{% endhighlight %}

----

## 2. Create new static-site with a built-in template

{% highlight sh linenos %}
$ cd /path/to/your/static-website/
$ bundle exec jekyll new yourprojectnameishere
{% endhighlight%}

----

## 3. Run built-in server in order to check your new static site

{% highlight sh linenos %}
$ cd /path/to/your/static-website/
$ bundle exec jekyll serve --port 8080
{% endhighlight %}

Finally, access to http://localhost:4040/ on your web browser.

[![IMAGE ALT TEXT HERE](/imgs/jekyll1.png)](/imgs/jekyll1.png)

----

That's all.

