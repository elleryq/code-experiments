---
title: "C# Select / Python map"
date: 2013-10-14T02:51:00+08:00
draft: false
---
c# 的 LINQ select 大略等同於 Python 的 map ，用法稍嫌麻煩。
```
using System;
using System.Linq;
using System.Collections.Generic;

public class Proram {
    public static void Main(string[] args) {
        IEnumerable<int> values = new List<int>() {0, 1, 2, 3, 4};
        var x = values.Select(i => {
                      Console.WriteLine(i);
                      return i;  // 一定要傳回整數
                      });
        foreach(var i in x) {}  // 因為 Select 後的結果是 lazy 的，意即只有在真正去 iterate 時才會執行。
    }
}
```

```
# Python map example.
# 這邊使用了 __future__ ，因為在 Python 2 裡，print 是敘述，無法在 lambda 裡使用。
# 但在 Python 3 裡，print 是函式，因此可以
from __future__ import print_function
values = range(5)
map(lambda i: print(i), values)
```

參考資料：
 * [2. Built-in Functions — Python v2.7.5 documentation](http://docs.python.org/2/library/functions.html#map "2. Built-in Functions — Python v2.7.5 documentation")
 * [python lambda with print statement - Stack Overflow](http://stackoverflow.com/questions/2970858/python-lambda-with-print-statement "python lambda with print statement - Stack Overflow")
 * [C#/Linq: Apply a mapping function to each element in an IEnumerable? - Stack Overflow](http://stackoverflow.com/questions/7187409/c-linq-apply-a-mapping-function-to-each-element-in-an-ienumerable "C#/Linq: Apply a mapping function to each element in an IEnumerable? - Stack Overflow")
 * [c# - Select fails to print to console - Stack Overflow](http://stackoverflow.com/questions/14981876/select-fails-to-print-to-console "c# - Select fails to print to console - Stack Overflow")
 