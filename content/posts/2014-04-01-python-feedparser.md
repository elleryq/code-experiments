---
layout: post
title: 'Python feedparser'
date: 2014-04-01 02:59
comments: true
categories: 
---
feedparser 是一個可以用來解析 RSS/ATOM feed 的模組，是目前最常見也最常用的模組。用 pip search 時，有發現相似的模組 speedparser，號稱比 feedparser 更快，有機會再來試用看看。

```
from __future__ import print_function
import sys
import feedparser

# 會自動去抓取指定網址的 RSS/ATOM 並進行解析
d = feedparser.parse("http://your_site/rss.xml")

# 主要的條目都在 entries 裡
if 'entries' not in d:
    print("No entries")
    sys.exit(-1)

for entry in d['entries']:
    print("url:" + entry['link']) # 文章的網址
    print("title: " + entry['title'].encode('utf-8')) # 文章標題
```
