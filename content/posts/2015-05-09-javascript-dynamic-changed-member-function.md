---
layout: post
title: 'Javascript 動態改變 member function'
date: 2015-05-09 14:59
comments: true
categories: [javascript]
---
純粹因為懶得 fork 出來改，所以想說看能不能動態改變，算是 monkey patch 這樣。
實驗的結果是可以，畢竟這是動態語言應該要具有的能力。

```
"use strict";
var User = function() {
    this.className = "User";
};
User.prototype.findByUsername = function(username, cb) {  // 原來的函式
    console.log("findByUsername");
};
User.prototype.toString = function() {
    return this.className;
}

function attachToUser(UserSchema) {
    // 在這裡動態進行更換
    UserSchema.prototype.oldFindByUsername = UserSchema.prototype.findByUsername;
    UserSchema.prototype.findByUsername = function(username, cb) {
        console.log("new findByUsername");
    };
}

attachToUser(User);

var obj = new User();
obj.findByUsername("admin", function() {});
obj.oldFindByUsername("root", function() {});
console.log(obj.toString());
```

執行結果：
```
new findByUsername
findByUsername
User
```
