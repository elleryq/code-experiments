---
layout: post
title: 'C# about dynamic ExpandoObject member function'
date: 2014-07-22 09:48
comments: true
categories: 
---
由於 ExpandoObject 自身的能力，增加屬性非常簡單。但如果要增加一個 member function，該怎麼做？這可以透過 Func 或 Action 來達成。

```
List<dynamic> userlist = new List<dynamic>();

// 利用 currying 來模擬類似 python 的 foo(self, x, y...) 
// C++ 的 this 其實跟 python 一樣，但是 this 不需要明確的寫出來，這部分是編譯器幫忙做掉
// 在 C# 裡，看來是沒辦法，所以在設置 member function 時，就先把 instance 傳進 toString，
// 讓 toString 傳回另外一個新 function
Func<dynamic, Func<string> > toString = delegate(dynamic user) {
  return new Func<string> (() =>
    String.Format("User: {0} Age: {1}", user.Name, user.age)
  );
};

for(int i=0; i<100; i++) {
	dynamic user = new System.Dynamic.ExpandoObject();
	user.id = i;
	user.Name = String.Format("John{0}", i);
	user.age = i+10;
	user.toString = toString(user);  // 這裡就把 instance 先傳進去
	userlist.Add(user);
}

var userQuery = from user in userlist where user.age>79 select user;
Console.WriteLine("userQuery.count = {0}", userQuery.Count());
foreach( var user in userQuery ) {
  Console.WriteLine(user.toString());  // 使用的時候，就跟 C# 原用法相似。
}
```