---
layout: post
title: 'Python NodeTransformer'
date: 2013-10-23 02:44
comments: true
categories: 
---
透過 ast.NodeTransformer 可以動態的把程式裡的字串換掉，利用 compile() 再執行。
主要的說明就放在程式裡。

```
# transformer.py
from __future__ import print_function
import ast


class MyTransformer(ast.NodeTransformer):
    def visit_Str(self, node):  # 如果訪問到的 node 是 Str
        if node.s.startswith('Hello'): # Str 的內容以 'Hello' 起始
            s = node.s.replace('Hello', 'World')  # 換掉!!
            new_node = ast.copy_location(ast.Str(s=s), node)  # 透過 copy_location 複製資訊給新的 Str
        else:
            new_node = node  # 傳回原來的
        return new_node


if __name__ == "__main__":
    myprog = """
for i in range(10):
    print 'Hello {0}'.format(i)
    """  # 程式
    a = ast.parse(myprog)  # 拆解成 AST tree

    MyTransformer().visit(a)  # 訪問

    # 需額外安裝 codegen: pip install codegen
    # 可以看轉譯後的程式
    # import codegen
    # print(codegen.to_source(a))

    fixed = ast.fix_missing_locations(a)  # 重新修正位置，其實應該是可以不用
    codeobj = compile(fixed, "", 'exec')  # 編譯
    eval(codeobj)  # 執行!!
```