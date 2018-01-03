---
title: 'Python try-except-else-finally'
date: 2016-07-05T09:24:00+08:00
draft: false
categories: [python]
---
Python 的 try-except-finally 裡有 else 子句，這相當有意思。

finally 區塊的程式，不管 try-except 裡是否有 raise exception，都會被執行。而 else 則是只有在 try-except 裡沒有 raise exception 時才會被執行。

```
def foo(i):
    print("\ni={}".format(i))
    try:
        if i == 0:
            raise Exception("i == 0")
        print("Hello")
    except Exception, ex:
        print(ex)
    else:
        print("else")
    finally:
        print("finally")

foo(0)
foo(1)
```

執行結果
```
i=0
i == 0
finally

i=1
Hello
else
finally
```