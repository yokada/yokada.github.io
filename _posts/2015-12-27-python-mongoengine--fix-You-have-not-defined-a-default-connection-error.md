---
layout: post
title: 'python mongoengine: Fix "You have not defined a default connection" error'
---

## The problem

I coundn't to save a row via mongodb because of got a error that "You have not defined a default connection" message.

## Key points to fix

1. Add name of db alias to use `connect` function properly.

my data_mapper.py is here:
{% highlight python linenos %}
from mongoengine import *

class Repo(DynamicDocument):
  key = StringField(required=True)
  short_description = StringField(required=True)
  stars = IntField(required=True)

  meta = {"db_alias":"default", 'collection':'repo'}
{% endhighlight %}

See current file structure.
{% highlight python linenos %}
$ cd '/path/to/app'
$ ls .
data_mapper.py
{% endhighlight %}

Verify above python script via REPL like this:
{% highlight python linenos %}
$ python
>>> import sys
>>> sys.path.append('.')
>>> from data_mapper import Repo
>>> connect('mydb', 'default', host='localhost')
>>> Repo(key='mykey', short_description='my short description', stars=0).save()
{% endhighlight %}

Then, check mongodb that the row has been saved.
{% highlight sh linenos %}
$ mongo
$ use mydb
$ db.repo.find()
{ "_id" : ObjectId("567ed269420f214d88aeca36"), "key" : "key", "short_description" : "short", "stars" : 0 }
{% endhighlight %}

looks good.
