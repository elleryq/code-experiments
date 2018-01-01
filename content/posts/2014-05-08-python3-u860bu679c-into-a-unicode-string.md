---
layout: post
title: 'Python3 "\u860b\u679c" 轉為 Unicode 字串'
date: 2014-05-08 08:19
comments: true
categories: 
---
這是在 fb 上看到人家問的問題，可是後來好像貼的人又移除貼文了。

原作的輸入是在檔案裡，所以用 python 讀入以後，看到是 "\\u860b\\u679c" 這樣，想要轉為 unicode 字串。

我最先的想法是用 re 來找出符合 \u860b，折騰了半天，自己的 re 功力太差，寫的 pattern 不好，都沒 match 到。後來就想，直接拿 "\u" 來 split 就行了，於是：

```
input_string = "\\u860b\\u679c"
output = ''.join([chr(int(n, 16)) for n in s.split("\\u")[1:]])
```
