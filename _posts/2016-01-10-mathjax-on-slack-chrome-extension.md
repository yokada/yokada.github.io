---
layout: post
title: 'Using Mathjax on Slack with Chrome extension'
tags: mathjax
---

I refered Stackoverflow posts as following.
My contents.js version is here:

{% highlight javascript linenos %}
function LoadMathJax() {
  window.MathJax = null;
  if (!window.MathJax) {
    if (document.body.innerHTML.match(/$|\\\[|\\\(|<([a-z]+:)math/)) {
      var script = document.createElement("script");
      script.type = "text/javascript";
      script.src = "//cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML";
      script.text = [
        "MathJax.Hub.Config({",
        "tex2jax: {inlineMath: [['$','$'],['\\\\\(','\\\\\)']]}",
        "});"
      ].join("\n");
      var parent = (document.head || document.body || document.documentElement);
      parent.appendChild(script);
    }
  }
}

var script = document.createElement("script");
script.type = "text/javascript";
script.text = "(" + LoadMathJax + ")()";
var parent = (document.head || document.body || document.documentElement);
setTimeout(function () {
  parent.appendChild(script);
  parent.removeChild(script);
},1000);
{% endhighlight %}

----

It would be get you the result.

[![IMAGE ALT TEXT HERE](/imgs/slack-mathjax.png)](/imgs/slack-mathjax.png)

----

The download link is my complete zip archive that contains Chrome extension: 
[/resources/slack_mathjax.zip](/resources/slack_mathjax.zip)

----

## References

- [ r - Mathjax support in github using a Chrome browser plugin? - Stack Overflow ]( http://stackoverflow.com/questions/11255900/mathjax-support-in-github-using-a-chrome-browser-plugin )
