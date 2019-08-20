---
title: "Python asyncio"
date: 2019-08-20T15:00:00+08:00
draft: false
---
看 Linux Journal 時看到的，順便練一下。這篇只是前導，還沒到 asyncio 的新語法。不過後續文章可能不會繼續在 Linux journal 上刊登了，因為 Linux journal 公告停刊了。

* [Understanding Python's asyncio](https://www.linuxjournal.com/content/understanding-pythons-asyncio)

```
#!/usr/bin/env python
# -*- coding: utf-8 -*-

# 回傳結果是個 generator
def hello(name):
    while True:
        name = yield f'Hello, {name}'
        if not name:  # 當 name 為空時，就停止
            break


try:
    g = hello('world')    # 取得 genrator
    print(next(g))        # 用 next 取得下個結果，得到 "Hello, world"
    print(g.send('foo'))  # 用 send 送 "foo" 進去，得到 "Hello, foo"
    print(g.send('bar'))  # 再用 send 送 "bar" 進去，得到 "Hello, bar"
    print(g.send(''))     # 送空白進去，停止 generator，其實不用送這行也可以。
except StopIteration as ex:  # 當 hello() 裡的 while 離開時，會 raise StopIteration
    print(ex)
    pass
```
