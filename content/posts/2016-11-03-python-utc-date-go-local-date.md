---
title: 'Python - UTC 日期轉 local 日期'
date: 2016-11-03T08:25:00+08:00
draft: false
categories: [python]
---
用 dateutil 做解析日期字串，然後用 arrow 做轉換，很方便。

```
from __future__ import print_function
import sys
from dateutil.parser import parse
import arrow


def utc2local(utc_date_str):
    utc_date = parse(utc_date_str)  # 用 dateutil.parser.parse 來解析日期字串
    local_date = arrow.get(utc_date).to('Asia/Taipei')  # 拿到日期，用 to 轉換，收工
    return local_date.format('YYYY-MM-DD HH:mm:ss')  # 格式化以後傳回


def main():
    if not len(sys.argv[1:]):
        print("No arguments")
        return
    for arg in sys.argv[1:]:
        print(utc2local(arg))


if __name__ == "__main__":
    main()
```