---
layout: post
title: 'python mongoengine: Fix "You have not defined a default connection" error'
tags: python
---

## The problem

I coundn't to save a row via mongodb because of got a error that "You have not defined a default connection" message.

## Key points to fix

1. Add name of db alias to use `connect` function properly.

my data_mapper.py is here:

```python
from mongoengine import *

class Repo(DynamicDocument):
  key = StringField(required=True)
  short_description = StringField(required=True)
  stars = IntField(required=True)

  meta = {"db_alias":"default", 'collection':'repo'}
```

See current file structure.

```python
$ cd '/path/to/app'
$ ls .
data_mapper.py
```

Verify above python script via REPL like this:

```python
$ python
>>> import sys
>>> sys.path.append('.')
>>> from data_mapper import Repo
>>> connect('mydb', 'default', host='localhost')
>>> Repo(key='mykey', short_description='my short description', stars=0).save()
```

Then, check mongodb that the row has been saved.

```shell
$ mongo
$ use mydb
$ db.repo.find()
{ "_id" : ObjectId("567ed269420f214d88aeca36"), "key" : "key", "short_description" : "short", "stars" : 0 }
```

looks good.
