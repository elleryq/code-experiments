---
layout: post
title: 'Python nested if'
date: 2014-06-18 02:21
comments: true
categories: [Python]
---
之前在 checkio 網站有看過人家這樣寫：
```
def checkio(x, y):
  result = (x>10, y<10)
  conditions = {
    (True, True): 1,
    (True, False): 2,
    (False, True): 3,
    (False, False): 4 }
  return conditions(x, y)
```

真的有醍醐灌頂之感，原來 tuple 也可以拿來當作 dict 的 key。

不過今天不是說這個，昨天天寫出這種 code，覺得超級醜：
```
s = "Hello"
pos = s.find('a')
if pos == -1:
  pos = s.find('b')
  if pos == -1:
    pos = s.find('c')
    if pos == -1:
      return
```

今天就在想要怎麼寫會比較好，原本想說：
```
pos = s.find('a') or s.find('b') or s.find('c')
```
但是，-1 會被判定為 True，導致後面不會繼續進行判斷。

後來就想到可以利用 map 跟 dropwhile
```
from itertools import dropwhile
CHARS = ('a', 'b', 'c')
s = "Hello"
poses = list(dropwhile(lambda x: x == -1,
                       map(lambda x: s.find(x), CHARS)))
if not poses:
  return
pos = poses[0]
```
雖然會因為 map 的關係，find 被執行了好幾次，但是卻讓程式簡短不少，而且如果要增加要搜索的字元的話，只要修改 CHARS 就可以。

是說，好像也可以利用 re 模組的 or 來處理。
