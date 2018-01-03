---
title: 'Python 多重繼承'
date: 2014-03-14T08:20:00+08:00
draft: false
---
在 Facebook 上看到有人問，所以就來試驗看看。原 PO 的問題是問，當多重繼承時，在子類別的 ctor() 使用 super() 去呼叫父類別的 ctor()，為什麼只有呼叫到左邊父類別的 ctor()。好吧，其實我不知道為什麼，這可能要看 super() 是如何實作的，但我試驗以後，認為是要利用__bases__來處理，__bases__ 會傳回所有的父類別，以下面的例子來說，C.__bases__ 就會是 (A, B)，如此一來，就可以利用它來呼叫到每個父類別的 ctor()。

```
# -*- coding: UTF8 -*-


class A(object):
    def __init__(self, parent=None):
        print('init A class')


class B(object):
    def __init__(self, parent=None):
        print('init B class')


class C(A, B):
    def __init__(self, parent=None):
        for clazz in self.__class__.__bases__:  # 改用 __bases__
            clazz.__init__(self, parent)
        #super(self.__class__, self).__init__(parent)  # 只呼叫了 A.__init__()
        print('init C class')

c = C()
```