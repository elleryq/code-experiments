---
title: 'Python - metaclass'
date: 2017-06-26T09:12:00+08:00
draft: false
categories: [python]
---
好奇 Django 的 Meta 怎麼做的，上網找了資料，並且參考 Django 的原始
碼。

```
#!/usr/bin/env python
# -*- coding: utf-8 -*-
import six


# 先做一個 Meta class ，Django 的 ModelBase 就是一個 Meta class
class Meta(type):  # 一定要繼承 type
    def __call__(self):
        print('Enter Meta.__call__: {}'.format(self))
        obj = super(Meta, self).__call__()
        print('Exit Meta.__call__: {}'.format(obj))
        return obj

    # 主要的關建
    def __new__(cls, name, bases, attrs):
        print('Enter Meta.__new__: {} {} {} {}'.format(
            cls, name, bases, attrs))
        newClass = super(Meta, cls).__new__(cls, name, bases, attrs)
        
        # 取得類別裡 Meta 子類別的 attribute
        metaClass = attrs.pop('Meta')
        if hasattr(metaClass, 'app_config'):
            print('app_config={}'.format(getattr(metaClass, 'app_config')))
            
        print('Exit Meta.__new__: {}'.format(newClass))
        return newClass  # 回傳的類別會是 Animal 或 Animal3

    def __init__(cls, name, bases, dictionary):
        print('Enter Meta.__init__: {}'.format(cls, name, bases, dictionary))
        super(Meta, cls).__init__(name, bases, dictionary)
        print('Exit Meta.__init__')


# 指定 __metaclass__ ，在初始化 Animal object 時，會先呼叫 Meta.__new__ 做出真正的 Animal class
class Animal(object):
    __metaclass__ = Meta

    class Meta:
        app_config = "Practice"


# 另外一種指定 __metaclass__ 的方法，Django 是用 six.with_metaclass 來做。
class Animal3(six.with_metaclass(Meta)):
    class Meta:
        app_config = "Practice3"


def main():
    obj = Animal()
    print(obj)

    obj = Animal3()
    print(obj)

if __name__ == "__main__":
    main()
```