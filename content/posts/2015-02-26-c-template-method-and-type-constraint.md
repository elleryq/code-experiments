---
title: 'C# template method and type constraint'
date: 2015-02-26T08:34:00+08:00
draft: false
categories: [c#]
---
在 linqpad 裡試著，記得先選為 c# program，再貼 code

```
void Main()
{
	/*
	// show error
	run(20);
	run("Hello world");
	*/
	run(new Dictionary<string, string>());
}

// Define other methods and classes here
// Add type constraint for run()
// allow only IDictionary
void run<T>(T x) where T: IDictionary {
  Console.WriteLine(typeof(T).ToString());
}
```