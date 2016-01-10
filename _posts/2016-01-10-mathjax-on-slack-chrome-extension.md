---
layout: post
title: 'Using Mathjax on Slack with Chrome extension'
---

My contents.js is here:

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

It would be the result is here.

[![IMAGE ALT TEXT HERE](/imgs/slack-mathjax.png)](/imgs/slack-mathjax.png)
