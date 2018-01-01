---
title: "VBScript include/import"
date: 2013-10-14T10:20:00+08:00
draft: false
---
VBScript 沒有提供 include/import 的語法，難道就只能複製貼上了嗎？

Google 了一下，找到兩種解：
 * 利用 WSF ：WSF 是一個 XML 檔案，裡面描述會引用到哪些 script 檔案，執行這個 WSF 檔案，WScript 會幫你引用這些指定的 script 檔案。好處是可以同時含括 JScript 跟 VBScript 的檔案。
 * 利用 ExecuteGlobal 敘述：先把檔案內容都讀進來，再使用這個敘述去執行。這就僅限於 VBScript 了。
 
參考資料：
 * [How do I include a common file in VBScript (similar to C #include)? - Stack Overflow](http://stackoverflow.com/questions/316166/how-do-i-include-a-common-file-in-vbscript-similar-to-c-include)
 * [How to include a file from within a VBScript script](http://www.source-code.biz/snippets/vbscript/5.htm)
  