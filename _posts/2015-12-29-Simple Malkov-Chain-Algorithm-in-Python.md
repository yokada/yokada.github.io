---
layout: post
title: 'Simple Malkov Chain Algorithm in Python'
tags: python
---

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

from pprint import PrettyPrinter
import re
import random

pp = PrettyPrinter(indent=4)

#input_text = "On the 9th. William Findley and David Redick--deputed by the Committee of Safety (as it is designated) which met on the 2d. of this month at Parkinson Ferry arrived in Camp with the Resolutions of the said Committee; and to give information of the State of things in the four Western Counties of Pennsylvania to wit--Washington Fayette Westd. & Alligany in order to see if it would prevent the March of the Army into them."
input_text = """\
I am a programmer.
I am a Japanese.
"""

NOWORD = "\n"
w1, w2 = (NOWORD, NOWORD)
statetab = {}
statetab[(w1, w2)] = []

text = filter(lambda i:i != "", re.split(r"[\s]", input_text))
for t in text:
  if t == "":
    continue
  statetab[(w1, w2)].append(t)
  w1, w2 = (w2, t)
  if not (w1, w2) in statetab:
    statetab[(w1, w2)] = []

statetab[(w1, w2)].append(NOWORD)

pp.pprint(statetab)

w1, w2 = (NOWORD, NOWORD)
for i in range(len(text)):
  suf = statetab[(w1, w2)]
  r = random.randint(0, len(suf)-1)
  t = suf[r]
  if t == NOWORD:
    break
  print t,
  w1, w2 = (w2, t)
```

----

```shell
╰─➤  python malkov.py
{   ('\n', '\n'): ['I'],
    ('\n', 'I'): ['am'],
    ('I', 'am'): ['a', 'a'],
    ('a', 'Japanese.'): ['\n'],
    ('a', 'programmer.'): ['I'],
    ('am', 'a'): ['programmer.', 'Japanese.'],
    ('programmer.', 'I'): ['am']}
I am a Japanese.
```

