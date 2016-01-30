---
layout: post
title: "Sequential transition with nested callback functions in Corona SDK"
tags: "corona sdk"
---

{% highlight lua %}
local r = display.newRect(100, 100, 50, 50)
local function anim_seq(t, seq)
  if #seq > 0 then
    local q = table.remove(seq, 1)
    q.onComplete = function()
      anim_seq(t, seq)
    end
    transition.to(t, q)
  end
end
anim_seq(r, { {delta=true, time=500, x=100}, {delta=true, time=500, y=100}, {time=500, rotation=45}, {delta=true, x=-100, rotation=135}, {delta=true, y=-100, time=500} } )
{% endhighlight %}

