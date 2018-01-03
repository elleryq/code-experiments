---
title: 'Javascript setTimeout'
date: 2014-07-01T06:49:00+08:00
draft: false
categories: [javascript]
---
每次用每次都要查一下，這次要記下來。
有寫在 jsfiddle.net 那邊，網址是 http://jsfiddle.net/LaPFQ/
應該是要再包成類別之類的會比較好，不過暫時就這樣吧。

HTML
```
<div id="log"></div>
```

Javascript
```
var limit = 10;
var count = 0;

var trigger = function() {
    $("#log").append("triggered " + count + "<br/>");
    count++;
    if(count<=limit) {
        setTimeout(trigger, 2000);
        // $("#log").append("scheduled<br/>");
    }
};

$("#log").append("started<br/>");
var timeout = setTimeout(trigger, 2000);
```

