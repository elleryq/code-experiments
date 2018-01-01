---
layout: post
title: 'Python absolute_import'
date: 2014-04-16 03:52
comments: true
categories: 
---
在網路上看到 from \_\_future\_\_ import absolute_import ，不懂什麼意思，就研究一下，看是怎麼一回事。
主要定義在 PEP 328 http://legacy.python.org/dev/peps/pep-0328/

用例子來看 absolute_import 會比較清楚，這部份我是參考 http://bytes.com/topic/python/answers/596703-__future__-import-absolute_import 來試驗。

這次實驗需要的檔案比較多，一共7個。

```
# bar/__init__.py
# Empty
```

```
# bar/foo.py
print("inside bar")
```

```
# bar/absolute.py
from __future__ import absolute_import
import foo
```

```
# bar/relative.py
import foo
```

```
# foo.py
print("toplevel")
```

```
# main for testing absolute_import
import bar.absolute
```

```
# main for testing without absolute_import
import bar.relative
```

目錄架構是這樣的：
 * main_with_absolute_import.py
 * main_without_absolute_import.py
 * foo.py
 * bar
   * \_\_init\_\_.py
   * foo.py
   * absolute.py
   * relative.py

當執行 main_with_absolute_import.py 時，會看到 "toplevel"；執行 main_without_absolute_import.py 時，會看到 "inside bar"。也就是說，有 absolute_import 時，實際上 import 到的是當前目錄的 foo.py 而非 bar module 裡的 foo.py。