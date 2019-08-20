---
title: "Python property 能不能繼承"
date: 2019-05-28T17:00:00+08:00
draft: false
---
Python 的 property 在繼承之後，是有作用的。
```
#!/usr/bin/env python
# -*- coding: utf-8 -*-


class Base:
    @property
    def data(self):
        return {'Base': 'Base'}


class Child(Base):
    @property
    def data(self):
        # 取得父類別的 data property
        data = super(Child, self).data
        # 增加 "Child"
        data.update({'Child': 'Child'})
        return data


def main():
    base = Base()
    print("base.data={}".format(base.data))
    child = Child()
    print("child.data={}".format(child.data))


if __name__ == "__main__":
    main()
```