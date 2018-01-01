---
layout: post
title: 'C# 練習：仿 Java Lambda'
date: 2014-08-13 03:03
comments: true
categories: 
---
C# 從一開始仿 Java，到後來，語法上的發展已經超越 Java，所以現在反而是 Java 仿 C# 與其他語言。最近的 Java 8 在講的 Lambda ，早在 C# 2.0 時，就已經有了雛型，到了 C# 3.0 則已經發展成熟。

就拿這篇 [認識 Lambda/Closure（1）從 JavaScript 的函式物件談起 by caterpillar | CodeData](http://www.codedata.com.tw/java/understanding-lambda-closure-1-from-javascript-function-1 "認識 Lambda/Closure（1）從 JavaScript 的函式物件談起 by caterpillar | CodeData") 裡的例子來改寫：
```
// lambda1.cs
// How to build:
// Linux: dmcs /t:exe lambda1.cs
// Windows: csc /t:exe lambda1.cs
using System;
using System.Collections.Generic;

public class DelegatePractice {
    public static void Main(string[] args) {
        new List<string>(
            new string[] {"1", "2", "3", "4", "5"}
        ).ForEach(delegate(string s) {
            Console.WriteLine(s);
        });
    }
}
```