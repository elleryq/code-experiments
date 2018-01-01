---
layout: post
title: 'C# currying'
date: 2014-07-01 07:26
comments: true
categories: 
---
順手試試看。
C# 的 Func 比較囉唆一點，寫完你會很懷念 javascript 或 python 之類的語言。第一個參數是傳給函數的參數，第二個參數是傳回的參數。所以要 Currying，就是把第二個參數再設為 Func 就可以。Func 不只有一個參數的版本，也有兩個、三個...等等的版本，但不是無限的，有一定限度，最多到 16 個，也就是 Func\<T1, T2, T3...., T16, TResult> 。

```
using System;

public class Anonymous {
    public static void Main() {
        Func<string, Func<string, int> > say = delegate(string greeting) {
            Func<string, int> foo = delegate(string name) {
                Console.WriteLine("{0} {1}", greeting, name);
                return 0;
            };
            return foo;
        };

        Func<string, int> hello = say("Hello");
        hello("John");
    }
}
```