---
layout: post
title: "Github Pages could not be update a pushed post"
tags: github-pages
---

A few days ago, I pushed a post to my github pages; this site,repository using jekyll,<br>
but the post nevertheless had been gave me a 404 error.<br>
the below steps are what I did until to fix a bit of problem.

1. Checked my post contents.
2. Contacted to Github.
3. Do `bundle update` on your local jekyll due to update your gem `github-pages` if you using.
    - That's why `github-pages` gem have updated from `v2.4.0` to `v3.0.3`.

I did not know why the problem was fixed.

