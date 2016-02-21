---
layout: post
title: "Github User Pages could not be update a pushed post immediately"
tags: github-pages
---

A few days ago, I pushed a post to my github user pages (not project pages); this site,repository using jekyll,
but the post nevertheless had been gave me a 404 error.
the below steps are what I did until to fix a bit of problem.

1 Checked my post contents.
2 Checked my _config.yml.

the _config.yml was below:

```yml
baseurl:     /
exclude: [vendor]
excerpt_separator: "" # workarround for "highlight tag was never closed error"
gems:
  - jekyll-sitemap
```

then, changed it as following:

```yml
baseurl:     /
exclude: [vendor]
gems:
  - jekyll-sitemap
highlighter: rouge
markdown: kramdown
kramdown:
  input: GFM
```

3 Contacted to Github to ask about this problem.
4 Do `bundle update` on your local jekyll due to update your gem `github-pages` if you using. That's why `github-pages` gem have updated from `v2.4.0` to `v3.0.3`.
5 Waiting to reflect changed.

I realized new post to pushed then it will take about 1 or 2 days over.
I did not know why the update take long times to reflect the changes so like that.

## Resources

- [ Troubleshooting Jekyll builds - User Documentation ]( https://help.github.com/articles/troubleshooting-jekyll-builds/ )

