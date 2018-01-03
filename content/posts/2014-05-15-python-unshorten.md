---
title: 'Python unshorten'
date: 2014-05-15T03:13:00+08:00
draft: false
---
在 Stackoverflow 上找到這兩篇：
 * [curl - How can I un-shorten a URL using python? - Stack Overflow](https://stackoverflow.com/questions/7153096/how-can-i-un-shorten-a-url-using-python "curl - How can I un-shorten a URL using python? - Stack Overflow")
 * [How can I unshorten a URL using python? - Stack Overflow](https://stackoverflow.com/questions/4201062/how-can-i-unshorten-a-url-using-python "How can I unshorten a URL using python? - Stack Overflow")
 
原理很簡單，就是送 HEAD request 出去，然後判斷回傳的 status code；如果是 30x，表示是轉址，就拿取得的 header 裡的 location 繼續去查；如果是 20x，就表示拿到原來的網址了。

後來是用了第一篇的程式碼，程式是使用遞迴去處理網址是轉址的情況。
```
def unshorten_url(url):
    parsed = urlparse.urlparse(url)
    h = httplib.HTTPConnection(parsed.netloc)  # 這邊是用 httplib，你可以用你熟悉的 module
    resource = parsed.path
    if parsed.query != "":
        resource += "?" + parsed.query
    h.request('HEAD', resource)  # 送 HEAD
    response = h.getresponse()
    if response.status/100 == 3 and response.getheader('Location'):  # 處理 status code 為 3 的情況
        # changed to process chains of short urls
        return unshorten_url(response.getheader('Location'))  # 遞迴
    else:  # 回傳網址
        return url
```
