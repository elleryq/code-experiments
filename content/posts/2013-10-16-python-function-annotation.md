---
title: "Python function annotation"
date: 2013-10-16T07:37:00+08:00
draft: false
---
在 Python cookbook 7.3 看到 Function annotation ，在 Python 2 上卻不行，後來想到在 Python 3 上面試，果然就可以了。

```
def foo(i: int) -> int:
  return i*2
  
foo(2)
```

這是被規範在 [PEP 3107 -- Function Annotations](http://www.python.org/dev/peps/pep-3107/) ，我想目前用的人應該不多吧，在未來一兩年內應該也不多，畢竟大家多少還是考慮到跟 Python 2 的相容性，相信等到 Python 3 普及以後，這會被大量使用到。

參考資料：
 * [A powerful unused feature of Python: function annotations. | Manuel Cerón](http://ceronman.com/2013/03/12/a-powerful-unused-feature-of-python-function-annotations/)