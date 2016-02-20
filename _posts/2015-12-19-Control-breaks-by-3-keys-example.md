---
layout: post
title: Control breaks by 3 keys
tags: php
---

## master.csv

  0,2,4
  1,1,1
  1,1,5
  1,1,9
  1,3,3
  2,2,0
  2,2,2
  2,5,1
  2,6,3
  2,8,8
  2,9,0
  3,1,3

## test01.csv

  0,0,9
  0,1,3
  0,2,3
  0,5,1
  1,1,1
  1,1,2
  1,2,3
  2,2,1
  2,2,2
  3,0,1
  3,1,2


## main.php

```php
<?php

$master = fopen('./master.csv', 'r');
$input = fopen('./test01.csv', 'r');
$s1dup = fopen('./step.out', 'w');
$s1uniq = fopen('./step2.out', 'w');

$prevOffset = 0;
$last;
while(($buf = fgets($master)) !== false) {
  $p = explode(",",trim($buf));
  echo implode("\t", $p), PHP_EOL;
  while(($buf2 = fgets($input)) !== false) {
    $last = null;
    $q = explode(",", trim($buf2));
    echo "\t", implode("\t", $q), PHP_EOL;
    if($p[0] == $q[0] and $p[1] == $q[1] and $p[2] == $q[2]) {
      echo "match", PHP_EOL;
      fwrite($s1dup, implode("\t", $q) . PHP_EOL);
      $prevOffset = ftell($input);
      $last = 'm';
      break;
    } elseif ($p[0] == $q[0] and $p[1] == $q[1] and $p[2] < $q[2]) {
      echo "break3", PHP_EOL;
      fwrite($s1uniq, implode("\t", $q) . PHP_EOL);
      fseek($input, $prevOffset);
      $last = 'b';
      break;
    } elseif ($p[0] == $q[0] and $p[1] < $q[1]) {
      echo "break2", PHP_EOL;
      fwrite($s1uniq, implode("\t", $q) . PHP_EOL);
      fseek($input, $prevOffset);
      $last = 'b';
      break;
    } elseif ($p[0] < $q[0]) {
      echo "break1", PHP_EOL;
      fwrite($s1uniq, implode("\t", $q) . PHP_EOL);
      fseek($input, $prevOffset);
      $last = 'b';
      break;
    }
    $prevOffset = ftell($input);
  }

  if(is_null($last)) {
    fwrite($s1uniq, implode("\t", $p) . PHP_EOL);
  }
}
```

## The result

  $ php main.php
  0  2  4
    0  0  9
    0  1  3
    0  2  3
    0  5  1
  break2
  1  1  1
    0  5  1
    1  1  1
  match
  1  1  5
    1  1  2
    1  2  3
  break2
  1  1  9
    1  2  3
  break2
  1  3  3
    1  2  3
    2  2  1
  break1
  2  2  0
    2  2  1
  break3
  2  2  2
    2  2  1
    2  2  2
  match
  2  5  1
    3  0  1
  break1
  2  6  3
    3  0  1
  break1
  2  8  8
    3  0  1
  break1
  2  9  0
    3  0  1
  break1
  3  1  3
    3  0  1
    3  1  2

  $ lv step.out
  1       1       1
  2       2       2

  $ lv step2.out
  0       5       1
  1       2       3
  1       2       3
  2       2       1
  2       2       1
  3       0       1
  3       0       1
  3       0       1
  3       0       1
  3       1       3

That's all.

