---
layout: post
title: 'Python - 將句子中的單字首字轉換成大寫'
date: 2016-10-21 01:20
comments: true
categories: [python]
---
在網路上看到，就順便試試看。來源：[Let's Start From Here: [JS/ES6] 將句子中的單字首字轉換成大寫-不用正規表示法](https://zerotowhole.blogspot.tw/2016/10/jses6_20.html "Let's Start From Here: [JS/ES6] 將句子中的單字首字轉換成大寫-不用正規表示法")

用 regular expression 的話，應該是可以用 \w+ 去找到每個 match，再用 capitalize() 轉換就行了。
```
# -*- coding: utf-8 -*-
"""Convert each words capital in string."""
from __future__ import print_function
from argparse import ArgumentParser


def convert_each_word_capital(s):
    """Convert each words capital in string."""
    words = s.split(" ")
    capitaled_words = map(lambda x: x.capitalize(), words)
    return ' '.join(capitaled_words)


def main():
    """Main."""
    parser = ArgumentParser()
    parser.add_argument('strings', metavar="S", type=str,
                        nargs='+')
    args = parser.parse_args()

    for arg in args.strings:
        print("---")
        print("Before: {}".format(arg))
        result = convert_each_word_capital(arg)
        print("After: {}".format(result))


if __name__ == "__main__":
    main()
```

