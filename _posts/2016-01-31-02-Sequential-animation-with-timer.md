---
layout: post
title: "Sequential animation with timer"
tags: coronasdk
---

{% highlight lua %}
-- anim.lua
module(..., package.seeall)
local m = {}
local cur = 1
local anim_q = {}
local anim_timers = {}
local anim_running = false
function m.seq(t, seq, p, is_last)
  if t.seq == nil then
    t.seq = table.copy(seq)
  end
  p = p or {}
  if #seq >= 0 then
    local q = table.remove(seq, 1)
    if q ~= nil then
      q.onComplete = function()
        if #seq == 0 then
          if type(p.loop) == 'number' then
            p.loop = p.loop - 1
            if p.loop > 0 then
              seq = table.copy(t.seq)
            end
          elseif type(p.loop) == 'boolean' and p.loop then
            seq = table.copy(t.seq)
          elseif type(p.onComplete) == 'function' then
            p.onComplete()
          end
          if is_last then
            timer.cancel(anim_timers[cur])
            cur = cur + 1
            anim_running = false
            m.await_timer()
          end
        end
        m.seq(t, seq, p, is_last)
      end
      transition.to(t, q)
    end
  end
end
function m.await_timer()
  local timer_id = timer.performWithDelay(10, function()
    if anim_running == false then
      anim_running = true
      local params = anim_q[cur]
      if params ~= nil then
        for i, v in ipairs(params) do
          v[3] = v[3] or {}  -- additional properites
          v[1].seq = table.copy(v[2])
          local is_last = (i == #params)
          m.seq(v[1], v[2], v[3], is_last)
        end
      end
    end
  end)
  table.insert(anim_timers, timer_id)
end
function m.await(...)
  local params = {...}
  table.insert(anim_q, params)
  m.await_timer()
end

return m
{% endhighlight %}

----

How to use `seq` function:

{% highlight lua %}
-- main.lua

local anim = require 'anim.lua'

local r  = display.newRect(100, 100, 50, 50)

anim.seq(r, { {delta=true, time=500, x=100}, {delta=true, time=500, y=100}, {time=500, rotation=45}, {delta=true, x=-100, rotation=135}, {delta=true, y=-100, time=500} } )
{% endhighlight %}

----

And also use `await` function:

{% highlight lua %}
-- main.lua

local anim = require 'anim.lua'

local r  = display.newRect(100, 100, 50, 50)
local r2 = display.newRect(200, 100, 50, 50)

anim.await(
  --{ r,  { {delta=true, rotation=90, time=500} }, {loop=5} },
  --{ r,  { {delta=true, rotation=90, time=500} }, {loop=true} },
  { r,  { {delta=true, rotation=90, time=500} }, {onComplete=function() print('hoge') end} },
  { r2, { {rotation=90, time=500}, {delta=true, y=100, time=500} } }
)
anim.await(
  { r,  { {delta=true, rotation=90, time=500}, {delta=true, y=90, time=500} } },
  { r2, { {rotation=90, time=500}, {delta=true, y=100, time=500} } }
)
{% endhighlight %}

[![IMAGE ALT TEXT HERE](/imgs/20160131_02.gif)](/imgs/20160131_02)
