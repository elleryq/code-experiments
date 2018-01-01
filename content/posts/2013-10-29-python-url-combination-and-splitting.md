---
layout: post
title: 'Python URL 組合與分拆'
date: 2013-10-29 03:40
comments: true
categories: 
---
Python 2 裡是用 urlparse

```
# 組合
from urlparse import urljoin
from urllib import urlencode
base_url = "http://something.com"
users_url = urljoin(base_url, "users")  # 得到 http://something.com/users
params = {"_id": 0, "name": "Guest"}
print urljoin(users_url, '?' + urlencode(params)) # 這個 '?' 得自己加，參考資料 3 裡有不錯的函式可以參考
```

```
# 分拆
from urlparse import urlparse, parse_qs
url = 'http://something.com?blah=1&x=2'
print urlparse(url).query  # 得到'blah=1&x=2'
print parse_qs(urlparse(url).query) # 得到 {'blah': ['1'], 'x': ['2']}
```

到了 3.0 ，得改用 urllib.parse 模組，使用方法大同小異，就是 import 的地方要修改
```
from urllib.parse import urlencode
from urllib.parse import urljoin
from urllib.parse import parse_qs
```

參考資料：
1. [django - Best way to get query string from a URL in python? - Stack Overflow](http://stackoverflow.com/questions/11280948/best-way-to-get-query-string-from-a-url-in-python "django - Best way to get query string from a URL in python? - Stack Overflow")
2. [20.16. urlparse — Parse URLs into components — Python v2.7.5 documentation](http://docs.python.org/2/library/urlparse.html "20.16. urlparse — Parse URLs into components — Python v2.7.5 documentation")
3. [Add params to given URL in Python - Stack Overflow](http://stackoverflow.com/questions/2506379/add-params-to-given-url-in-python "Add params to given URL in Python - Stack Overflow")
 