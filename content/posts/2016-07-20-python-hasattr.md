---
title: 'Python hasattr'
date: 2016-07-20T02:36:00+08:00
draft: false
categories: [python]
---
今天看到這篇文章：[一个危险的Python函数，不推荐使用| 编程派 | Coding Python](http://codingpy.com/article/hasattr-a-dangerous-misnomer/ "一个危险的Python函数，不推荐使用")

裏面說，在 Python 2 裡不要用 hasattr 來判斷是否有屬性存在，好奇做了實驗。

我使用的 Python2 是 2.7.6，Python 3 是 3.4.3

```
# test_hasattr.py
# python2 有繼承 object 跟沒繼承 object 有差別，所以特別試試看。
# 沒繼承 object
class B():
  def __init__(self):
      self.name = "B"

  @property
  def y(self):
    # print("y")
    return "y"


# 有繼承 object
class C(object):
  def __init__(self):
      self.name = "B"

  @property
  def y(self):
    # print("y")
    return "y"

print("hasattr(B(), 'y')={}".format(hasattr(B(), "y")))
print("hasattr(B(), 'name')={}".format(hasattr(B(), "name")))

print("hasattr(C(), 'y')={}".format(hasattr(C(), "y")))
print("hasattr(C(), 'name')={}".format(hasattr(C(), "name")))
```

用 python2 執行：
```
hasattr(B(), 'y')=True
hasattr(B(), 'name')=True
hasattr(C(), 'y')=True
hasattr(C(), 'name')=True
```

用 python3 執行：
```
hasattr(B(), 'y')=True
hasattr(B(), 'name')=True
hasattr(C(), 'y')=True
hasattr(C(), 'name')=True
```

都可以正確的判別是否有指定的屬性。**另外一件有趣的事情是，在使用 hasattr() 時，會執行 property getter 函式。**

最後試著找文章裡提到的原文，發現已經無法瀏覽了。
