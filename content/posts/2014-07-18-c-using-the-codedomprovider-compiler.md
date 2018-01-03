---
title: 'C# 使用 CodeDomProvider 編譯程式'
date: 2014-07-18T03:14:00+08:00
draft: false
categories: [c#]
---
也是從 metaprogramming in .net 看來的，一樣是在 ubuntu 12.04 上用 dmcs /t:exe intantiatingCodeProviders.cs 編譯，執行沒問題。書裡說會有找不到 csc.exe 的情況，但至少在 Ubuntu 上是沒看到。

```
// intantiatingCodeProviders.cs
using Microsoft.CSharp;
using System.CodeDom.Compiler;
using System.Collections.Generic;

class IntantiatingCodeProviders {
    static void Main() {
        var providerOptions = new Dictionary<string, string>();
        providerOptions.Add("CompilerVersion", "v4.0");
        var csProv = new CSharpCodeProvider(providerOptions);
        var compilerParameters = new CompilerParameters(new string[] { });
        CompilerResults results = csProv.CompileAssemblyFromSource(compilerParameters,
            @"namespace V3Features
            {
                class Program {
                    static void Main() {
                        var name = ""Kevin"";
                        System.Console.WriteLine(name);
                    }
                }
            }");
    }
}
```

好，編譯好之後，該怎麼調用裏面的類別呢??
所以針對上面的程式，做了一些延伸，從 CompilerResult 這裡可以取得 Assembly，接著就可以套用 Reflection 的知識，從 Assembly 裡取得 Type，再從 Type 裡取得 Method(GetMethod)、成員(GetMember)，這樣就能呼叫或存取裏面的成員或函式了。
```
using System;
using System.Reflection;
using Microsoft.CSharp;
using System.CodeDom.Compiler;
using System.Collections.Generic;

class IntantiatingCodeProviders {
    static void Main() {
        var providerOptions = new Dictionary<string, string>();
        providerOptions.Add("CompilerVersion", "v4.0");
        var csProv = new CSharpCodeProvider(providerOptions);
        var compilerParameters = new CompilerParameters(new string[] { });
        CompilerResults results = csProv.CompileAssemblyFromSource(compilerParameters,
            @"using System;
            namespace V3Features
            {
                public class Program {
                    static void Main() {
                        var name = ""Kevin"";
                        System.Console.WriteLine(name);
                    }
                }

                public class Animal {
                    public void Bark(string greeting) {
                        Console.WriteLine(greeting);
                    }
                }
            }");

        if(results.Errors.Count>0) {
            foreach(CompilerError error in results.Errors) {
                Console.WriteLine("{0}:{1}", error.Line, error.ToString());
            }
            return;
        }

        Assembly assembly = results.CompiledAssembly;
        if(assembly==null) {
            Console.WriteLine("No compiled assembly.");
            return;
        }

        Type t = assembly.GetType("V3Features.Animal");
        if(t==null) {
            Console.WriteLine("Cannot get type.");
            return;
        }

        object obj = Activator.CreateInstance(t);
        if(obj!=null) {
            Console.WriteLine("create ok.");
            MethodInfo mi = t.GetMethod("Bark");  // 得用 reflection
            mi.Invoke(obj, new object[] {"Hello"});
        }
        else {
            Console.WriteLine("Cannot create instance.");
        }
    }
}
```

如果把後面的 object obj = Activator.CreateInstance(t); 改為使用 dynamic 的話，就可以不用麻煩的 reflection 了。
```
        dynamic obj = Activator.CreateInstance(t);
        if(obj!=null) {
            obj.Bark("Hello");  // 直接使用!!
        }
        else {
            Console.WriteLine("Cannot create instance.");
        }
```
