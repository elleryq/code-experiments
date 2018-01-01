---
title: "VBScript isNumeric 跟你想的不一樣"
date: 2013-10-14T10:08:00+08:00
draft: false
---
原本以為 isNumeric 是可以信賴的，豈知 Google 以後，發現真的是有問題：[Don’t trust a string based on TryParse or IsNumeric result! (.Net/VBScript) | Computer Security Is My Interest!](http://soroush.secproject.com/blog/2012/10/dont-trust-a-string-based-on-tryparse-or-isnumeric-result-netvbscript/)

但實驗結果跟該文章的結果不同：
```
1 True
$10 False
1,,2,,,3,, True
-10 True
-10 True
10- True
1.00E+02 True
%20%091 False
1%20%00%00 False
%26hff False
%0B%09%20-0001,,,,2.8e0002%09%20%0C%00%00 False
%0B$%09%20(0001,,,,2.8e0002%09%20)%0C%00%00 False
```
看起來微軟也許有修正了，由於沒辦法比較作者的 VBScript 版本跟我的 VBScript 版本，我想還是自己用 regular expression 來擋會比較保險了。

驗證用程式：
```
' Please use cscript to run.
dim strs, result

strs = array( _
    "1", _
    "$10", _
    "1,,2,,,3,,", _
    "-10", _
    "-10", _
    "10-", _
    "1.00E+02", _
    "%20%091", _
    "1%20%00%00", _
    "%26hff", _
    "%0B%09%20-0001,,,,2.8e0002%09%20%0C%00%00", _
    "%0B$%09%20(0001,,,,2.8e0002%09%20)%0C%00%00" _
)

for each s in strs
    If isNumeric(s) then
        result = "True"
    Else
        result = "False"
    End if
    WScript.Echo StrFormat("{0} {1}", Array(s, result))
next

' Origin: http://stackoverflow.com/questions/1840767/vbscript-what-is-the-simplest-way-to-format-a-string
Public Function StrFormat(FormatString, Arguments())
    Dim Value, CurArgNum

    StrFormat = FormatString

    CurArgNum = 0
    For Each Value In Arguments
        StrFormat = Replace(StrFormat, "{" & CurArgNum & "}", Value)
        CurArgNum = CurArgNum + 1
    Next
End Function
```

補充：
 1. 用 wscript 或是在檔案總管裡點選執行的時候，會跳很多 popup 視窗，請改到 command prompt 用 cscript 來執行。
 2. 用 wscript.StdOut 輸出會有錯誤訊息，這也是因為沒有用 cscript 來執行的關係。詳情可以看 [Why does this VBScript give me an error? - Stack Overflow](http://stackoverflow.com/questions/774319/why-does-this-vbscript-give-me-an-error)