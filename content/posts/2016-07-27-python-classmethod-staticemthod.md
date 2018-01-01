---
layout: post
title: 'Python classmethod/staticemthod'
date: 2016-07-27 10:41
comments: true
categories: 
---
知道但是不太熟，特別來試試看。
classmethod 跟 staticmethod 很相似，classmethod 有帶一個 cls ，就是 class 本身，staticmethod 比較像是 c++/java 裡的 static。

```
from __future__ import print_function


class C(object):
    def __init__(self, name):
        self.name = name
        
    @classmethod
    def build(cls, arg1, arg2):
        print(cls)
        print(arg1)
        print(arg2)
        return cls("arg1={0} arg2={1}".format(arg1, arg2))

    @staticmethod
    def create():
        return C("Static")

c1 = C.build(3, 4)
print(c1.name)
c2 = C("John")
print(c2.name)
c3 = c2.build(1, 2)  # 也可以用 C.build(1, 2)
print(c3.name)
c4 = C.create()
print(c4.name)
```