---
layout: post
title: "Installing modules in Google App Engine for Python"
tags: googleappengine python
---

## Case 1. Third-party modules

If you want to installing third-party libraries (with no C extensions) which are not contained
on GAE Running environment like WTForms:

```shell
$ cd myapp
$ virtualenv venv
$ source venv/bin/activate
(venv) $ mkdir lib
(venv) $ pip install -t lib -r requirements.txt
```

or

```shell
(venv) $ pip install -t lib WTForms WTForms-Appengine
```

## Case 2. Modules that built-in Google App Engine

Else that libraries are contained in GAE running environment, then define that libraries by "libraries section" in app.yaml like so:

```shell
libraries:
- name: jinja2
  version: latest
```

