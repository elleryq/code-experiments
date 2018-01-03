---
title: 'Java Short'
date: 2015-02-03T03:24:00+08:00
draft: false
categories: [java]
---
沒想到簡單的 i + 1
```
class test_short {
  public void test() {
    Short i=1;
    i = i + 1;
  }
}
```
在形態是 Short 的情況下是會編譯失敗的。

得這樣改才行
```
class test_short {
  public void test() {
    Short i=1;
    i = (short)(i+1);
  }
}
```