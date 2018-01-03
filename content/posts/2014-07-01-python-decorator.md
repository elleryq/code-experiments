---
title: 'Python decorator'
date: 2014-07-01T01:38:00+08:00
draft: false
categories: [python]
tags: [python]
---
真的還是要自己試著憑空弄過一次才會比較清楚。

一般沒帶參數的 decorator，就是兩層。第一層負責接原來的函式，第二層的函式就是要傳回去的。有帶參數的 decorator，就是三層，第一層負責接參數，第二層負責接函式，第三層的函式是要傳回去的。

那如果有上多個 decorator 時，最裡層的會先被執行，接著才是外層的。

也可以用類別來寫，用類別寫就是要利用 \_\_call\_\_，之後再來試試看。

```
# -*- coding: utf-8 -*-
from __future__ import print_function, unicode_literals


def wrapper(f):
    def inside_func(*args, **kwargs):
        print("> wrapper")
        f(*args, **kwargs)
        print("< wrapper")
    return inside_func


def before(s):
    def new_func(f):
        def inside_func(*args, **kwargs):
            print(s)
            f(*args, **kwargs)
        return inside_func
    return new_func


def after(s):
    def new_func(f):
        def inside_func(*args, **kwargs):
            f(*args, **kwargs)
            print(s)
        return inside_func
    return new_func


def repeat(n):
    def new_func(f):
        def inside_func(s):
            for i in range(n):
                f(s)
        return inside_func
    return new_func


@wrapper
def foo(s):
    print(s)


@repeat(5)
def foo1(s):
    print(s)


@after("<<<")   # then here
@before(">>>")  # run first
def foo2(s):
    print(s)


foo("Hello")  # 前後會加上 '> wrapper' 與 '< wrapper'
foo1("World")  # 這會印五次 World，因為 repeat(5)
foo2("Hello world")  # 先印出 '>>>' ，執行原函式，再印出 '<<<'
```

輸出結果
```
> wrapper
Hello
< wrapper
World
World
World
World
World
>>>
Hello world
<<<
```