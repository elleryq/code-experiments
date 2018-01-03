---
title: 'Python URL 組合之二'
date: 2014-05-15T03:07:00+08:00
draft: false
---
urlparse.urlparse 解析後的結果，如果有想要變動的話，可以再用 urlparse.urlunparse 組回去，下面是一個從網址把 Google analytics 參數移除的例子。

```
import urlparse
from urllib import urlencode


def remove_ga_params(url):
    """把以 utm_ 開頭的參數都移除掉，utm_ 開頭的參數多半是 Google analytics 使用的"""
    parsed = urlparse.urlparse(url)  # 拆出來的 ParseResult 其實是 NamedTuple
    params = urlparse.parse_qs(parsed.query)  # 將字串型態的 query 轉為 dict 型態，以利處理
    for k in params.keys():
        if k.startswith("utm_"):
            del params[k]
    modified = list(parsed)  # 把 tuple 轉為 list，以便於修改
    modified[4] = urlencode(params)  # 4 => query  # 第四個是 query，這部份可以看 urlparse 模組說明
    return urlparse.urlunparse(modified)  # 用 urlunparse 轉回網址
```