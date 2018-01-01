---
title: "Classic ASP Security Project"
date: 2013-10-15T14:25:00+08:00
draft: false
---
[Classic ASP Security Project](https://www.owasp.org/index.php/Classic_ASP_Security_Project "Classic ASP Security Project") 這裡有針對 ASP 安全寫的一些 Library，主要有兩個：
 * SingerASP : 這個主要是檢查 request 用的，主要概念是透過 regex 。假定你的 default.asp 需要輸入某欄位，那麼就在 rules 下新增 default.svdl.asp ，在裡面寫規則，含括 stinger.asp 的 default.asp 再寫上：
``` 
Dim validator
set validator = new Stinger
'Send Javascript Code for better user experience, pass form name as parameter
response.write validator.getJavaScript ("form")
```
 就可以自動檢查輸入的欄位。rules/regex.xml 還看不出來在那邊使用到，裡面是寫了一堆 regex 的定義。
 * ClassicASP ESAPI : 這個則是一個用 C# 2005 寫的 COM 元件，給 ASP 呼叫用，呼叫的範例是 ClassicASP Default.asp。這個就比較包山包海，功能比較齊全。

這都有原始碼可以參考。

微軟自家也有 [ASP 安全中心](http://msdn.microsoft.com/en-us/library/ms525813%28v=vs.90%29.aspx "ASP安全中心")，也有專文介紹一定要檢查使用者的輸入：[Validating User Input to Avoid Attacks](http://msdn.microsoft.com/en-us/library/ms525361%28v=vs.90%29.aspx "Validating User Input to Avoid Attacks")
