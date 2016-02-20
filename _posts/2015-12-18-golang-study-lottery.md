---
layout: post
title: golang study lottery
tags: golang
---

I wrote lottery function to learn golang.

```go
package main

import (
  "math/rand"
  "fmt"
  "time"
)

func main() {
  var ret = make(map[int]float32)

  for i:=0;i<10000;i++ {
    ret[lottery()] += 1
  }
  fmt.Printf("ret[1]:%0.2f\n", ret[1] / 100)
  fmt.Printf("ret[2]:%0.2f\n", ret[2] / 100)
  fmt.Printf("ret[3]:%0.2f\n", ret[3] / 100)
  fmt.Printf("ret[4]:%0.2f\n", ret[4] / 100)
}

func lottery() int {
  // Probability model
  // 1 hit.  1/100 1, 1
  // 2 hit. 10/100 2, 11
  // 3 hit. 25/100 12, 36
  // miss.  64/100 37, 100
    rand.Seed( time.Now().UTC().UnixNano())
  p := [4]int{1,10,25,64}
  denomi := 100
  r := rand.Intn(denomi)
  //fmt.Printf("r:%d\n", r)
  switch {
  case r < p[0]: return 1
  case r < (p[0] + p[1]) - 1: return 2
  case r < (p[1] + p[2]) - 1: return 3
  default: return 4
  }
}
```

-----

To Build and run as following:

```shell
╰─➤  go build lottery.go
╰─➤  ./lottery
ret[1]:0.79
ret[2]:9.07
ret[3]:24.32
ret[4]:65.82
```

