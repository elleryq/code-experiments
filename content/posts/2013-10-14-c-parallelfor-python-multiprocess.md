---
title: "C# Parallel.For / Python multiprocess"
date: 2013-10-14T03:30:00+08:00
draft: false
---
Parallel 可以並行處理，並無法跟 map() 畫上等號。這跟 Python 的 multiprocess 比較相似。
```
// c# Parallel.For example.
using System;
using System.Threading.Tasks;

public class Program {
    public static void Main(string[] args) {
        Parallel.For(0, 5, i=> {  // 表示從 0~4，共五次。
                     Console.WriteLine(i.ToString());
                     });
    }
}
```

```
# Python multiprocess example.
import multiprocessing

def foo(i):
    print(i)
    
if __name__ == '__main__':
    pool = Pool()
    result = pool.map(foo, range(4))
```

參考資料：
 * [Parallel Class (System.Threading.Tasks)](http://msdn.microsoft.com/en-us/library/system.threading.tasks.parallel.aspx "Parallel Class (System.Threading.Tasks)")
 * [HOW TO：撰寫簡單的 Parallel.For 迴圈](http://msdn.microsoft.com/zh-tw/library/dd460713.aspx "HOW TO：撰寫簡單的 Parallel.For 迴圈")
 * [[C#]隨筆手扎 - Parallel &amp; Lock - 非非碼不可- 點部落](http://www.dotblogs.com.tw/codeman/archive/2011/08/10/32847.aspx#63311 "[C#]隨筆手扎 - Parallel &amp; Lock - 非非碼不可- 點部落")
 * [Python multiprocess] (http://docs.python.org/2/library/multiprocessing.html)
 