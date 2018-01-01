---
title: "Python imp"
date: 2013-10-30T03:43:00+08:00
draft: false
---
蠻好奇 Python 怎麼實現 Plugin，在 Stackflow 上找到 [Building a minimal plugin architecture in Python - Stack Overflow](http://stackoverflow.com/questions/932069/building-a-minimal-plugin-architecture-in-python "Building a minimal plugin architecture in Python - Stack Overflow") ，得知可以使用 imp 模組。
PyMOTW 有 imp 的範例：[imp – Interface to module import mechanism. - Python Module of the Week](http://pymotw.com/2/imp/ "imp – Interface to module import mechanism.")
這是 imp module 的說明文件：[30.1. imp — Access the import internals — Python v2.7.5 documentation](http://docs.python.org/2/library/imp.html "30.1. imp — Access the import internals")

如果你的 plugin 放在目錄下，最快的方法是把 plugin 目錄加到 sys.path 裡，這樣 find_module 就可以找到。
```
import sys
import os
import imp

sys.path.insert(1, os.path.join(os.getcwd(), 'plugins'))
f, fn, desc = imp.find_module('my_plugins')
try:
  package = imp.load_module('my_plugins', f, fn, desc)
except ImportError:
  print("Cannot import.")
finally:
  f.close()
```

如果要做到把 plugins 下的檔案都自動載入：
```
import sys
import os
import glob
import imp

PLUGINS_PATH = os.path.join(os.getcwd(), 'plugins')
packages = {}  # 預定都放在這個 dict 裡，以 module 名稱當作 key
sys.path.insert(1, PLUGINS_PATH)  # 設定路徑
for fn in glob.glob(os.path.join(PLUGINS_PATH, '*.py'):
  path, name = os.path.split(fn)
  if name == '__init__.py':  # 排除 __init__.py
    continue
  module_name, ext = name.split('.')  # 取得檔案名稱跟副檔名
  f, fn, desc = imp.find_module(module_name)  # 用 find_module 取得資訊
  packages[module_name] = imp.load_module(module_name, f, fn, desc)  # 載入!!
  
# 結果都在 packages 裡了
print(packages)
for k, v in packages.items():
  print(k, dir(v))
```

更進一步的，可以再強制要求 Plugins 裡的檔案都必須要繼承某類別或是設置某變數，藉此可以判斷 Plugin 的類型。