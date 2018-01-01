---
title: 'Pypy make_proxy'
date: 2013-10-17T19:51:00+08:00
draft: false
---
在 [What PyPy can do for your objects — PyPy 2.1.0 documentation](http://pypy.readthedocs.org/en/latest/objspace-proxies.html "What PyPy can do for your objects — PyPy 2.1.0 documentation") 看到 make_proxy 的範例：
```
from tputil import make_proxy

history = []
def recorder(operation):
    history.append(operation)
    return operation.delegate()

>>>> l = make_proxy(recorder, obj=[])
>>>> type(l)
list
>>>> l.append(3)
>>>> len(l)
1
>>>> len(history)
2
```

但實際卻會發生：
```
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/usr/lib/pypy/lib_pypy/tputil.py", line 29, in make_proxy
    tp = tproxy(type, perform)
TypeError: 'list' object could not be wrapped
```

參考 pypy 對 make_proxy 的測試，增加類別 A，並修改為
```
from tputil import make_proxy

class A(object):
  def append(self, item):
    pass

history = []
def recorder(operation):
    history.append(operation)
    return operation.delegate()

>>>> l = make_proxy(recorder, obj=[])
>>>> type(l)
<class '__main__.A'>
>>>> l.append(3)
>>>> len(history)
1
>>>> print history
[<ProxyOperation A.__getattribute__(<0x7f8f15887b50>, 'append')>]
```

這樣就可以了。make_proxy 是製造出一個代理物件，並且所有的操作都會呼叫 recorder 函式，範例裡，是呼叫 recorder 函式，把所有動作都紀錄到 history 裡。