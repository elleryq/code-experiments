---
layout: post
title: 'C# about dynamic ExpandoObject'
date: 2014-07-22 09:24
comments: true
categories: 
---
讀了 metaprogramming in .net 第一章，才知道 4.0 以後的 dynamic 很強大。

以前 c# 物件的成員，是必須先在類別裡指定好的，現在透過 ExpandoObject 就可以不需要了，直接就可以指定屬性，然後寫 linq 敘述時，就能直接使用。
```
List<dynamic> list = new List<dynamic>();

for(int i=0; i<100; i++) {
	dynamic container = new System.Dynamic.ExpandoObject();
	container.id = i;
	container.Name = String.Format("John{0}", i);
	container.age = i+10;
	list.Add(container);
}

var query = from obj in list where obj.age>55 select obj;
Console.WriteLine("query.count = {0}", query.Count());
```

補充兩個有趣的 library：
 * [Adventures with C# 4.0 dynamic - ExpandoObject, ElasticObject, and a Twitter client in 10 minutes](http://www.codeproject.com/Articles/62839/Adventures-with-C-dynamic-ExpandoObject-Elasti)
 * [The Dynamic Sugar Library ](http://frederictorres.blogspot.tw/2011/01/dynamic-sugar-library.html)