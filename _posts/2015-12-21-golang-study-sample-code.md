---
layout: post
title: golang study sample code 1
---

{% highlight go linenos %}
package main

import (
  "time"
  "fmt"
  "math/rand"
  "math"
)

type TimeEvent struct {
  From time.Time
  To time.Time
}

type TimeGap struct {
  From time.Time
  Duration int
}

func TestCreateSchedule(start time.Time, n int) []TimeEvent {
  var list []TimeEvent
  now := time.Now()
  rand.Seed(now.UTC().UnixNano())
  var f, t time.Time
  for i := 0; i < n; i++ {
    if i == 0 {
      f = start
    }
    t = f.Add(time.Duration(rand.Intn(120)+1) * time.Minute)
    list = append(list, TimeEvent{From: f, To: t})
    f = t.Add(time.Duration(rand.Intn(120)+1) * time.Minute)
    var _ = f
  }

  return list
}

func TimeAddRand(t time.Time, r int) time.Time {
  n := time.Unix(t.Unix(), 0)
  return n.Add(time.Duration(rand.Intn(r)+1) * time.Minute)
}

func FindRecommends(schedule []TimeEvent, minutes int, top int) []TimeEvent {
  rc := []TimeEvent{}
  now := time.Now()
  gaps := FindIntervals(schedule, now)
  //fmt.Printf("%v", gaps)
  for _, g := range gaps {
    sc := CreateScheduleByGap(g, minutes, top - len(rc), top)
    if len(sc) <= 0 {
      break
    }

    for _, s := range sc {
      rc = append(rc, s)
    }
  }

  return rc
}

func CreateScheduleByGap(timeGap TimeGap, minutes int, remain int, top int) []TimeEvent {
  list := []TimeEvent{}
  num := 0
  if timeGap.Duration == 0 {
    num = remain
  } else {
    num = int(math.Min(float64(remain), float64(timeGap.Duration / minutes)))
  }

  if num <= 0 {
    return list
  }

  from := timeGap.From
  for i:=0; i<num; i++ {
    to := from.Add(time.Duration(minutes) * time.Minute)
    list = append(list, TimeEvent{From: from, To: to})
    from = to
    var _ = from
  }
  return list
}

func FindIntervals(schedule []TimeEvent, now time.Time) []TimeGap {
  var gaps []TimeGap

  firstGap := int((schedule[0].From.Unix() - now.Unix()) / 60)
  if firstGap > 0 {
    gaps[0] = TimeGap{From: now, Duration: firstGap}
  }

  var from, to time.Time
  for i, s := range schedule {
    if i == 0 {
      from = s.To
      var _ = from
      continue
    }
    to = s.From
    minutes := int((to.Unix() - from.Unix()) / 60)
    if minutes > 0 {
      gaps = append(gaps, TimeGap{From: from, Duration: minutes})
    }

    if i > 0 {
      from = s.To
      var _ = from
    }

    if i >= len(schedule)-1 {
      gaps = append(gaps, TimeGap{From: from, Duration: 0})
    }
  }

  return gaps
}

func main(){
  schedule := TestCreateSchedule(time.Now(), 1000)
  //fmt.Printf("%v\n", schedule)
  rc := FindRecommends(schedule, 45, 10)
  fmt.Printf("%d, %v\n", len(rc), rc)
}

{% endhighlight %}

