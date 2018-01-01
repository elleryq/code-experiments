---
title: "Python list comprehension / map / multiprocess pool.map 比較"
date: 2013-10-16T08:40:00+08:00
draft: false
---
```
# 使用 for
def main():
    result = []
    for i in range(400):
        result.append(foo(i))
```

```
# 使用 list comprehension
def main():
    result = [foo(i) for i in range(400)]
```

```
# 使用 map
def main():
    result = map(foo, range(400))
```

使用 timeit 執行 1000000 次的結果：

|   for   | list comprehension |   map   |
| ------- | ------------------ | ------- |
| 96.8263 |      75.3605       | 77.5310 |

這裡的結果很奇妙，list comprehension 居然比較快。

把 range(400) 改為 range(1000000)，增加 multiprocessing ，並改用 cProfile 測試。

```
# 使用 multiprocessing
from multiprocessing import Pool

if __name__ == '__main__':
    pool = Pool()
    result = pool.map(foo, range(1000000))
```

|  for  | list comprehension |  map  | multiprocess | 
| ----- | ------------------ | ----- | ------------ |
| 0.610 |       0.406        | 0.383 |    0.299     |

結論就是，避免使用 for ，再視情況來用 list comprehension / map / multiprocessing 。

額外找到的資料，裏面都有介紹如何使用 timeit 跟 profiling ，同時也有幫畫圖的工具：
 * [A guide to analyzing Python performance « Huy Nguyen](http://www.huyng.com/posts/python-performance-analysis/ "A guide to analyzing Python performance « Huy Nguyen")
 * [2.4. Optimizing code — Scipy lecture notes](http://scipy-lectures.github.io/advanced/optimizing/ "2.4. Optimizing code — Scipy lecture notes")
 