---
layout: post
title: 'bluebird Promise'
date: 2015-05-12 10:29
comments: true
categories: 
---
sequelize 的 Promise 是用 bluebird 這個 package 實作的，就拿來練習。

```
var bluebird = require('bluebird');
var Promise = bluebird.Promise;

function async(url) {
    console.log("async");
    return new Promise(function(resolve, reject) {
        // 這邊簡單做，url 是 undefine 時，就呼叫 reject；否則就呼叫 resolve()
        // resolve() 就是走 then() 那路，reject() 就是走 catch() 那路。
        if(url) {
            resolve(url);
        }
        else {
            reject("url is empty");
        }
    });
}

async("http://localhost").then(function(url) {
    console.log("then " + url);
    return "new url";
}).then(function(url) {  // 2nd then() will get 1st then() returned value.
    console.log(url);    // url is 'new url'
}).catch(function() {
    console.log("catch");
}).finally(function() {
    console.log("finally");
});
/**
執行結果：
async
then http://localhost
new url
finally
*/

async().then(function(url) {
    console.log("then " + url);
    return "new url";
}).then(function(url) {
    console.log(url);
}).catch(function() {  // because async()'s parameter is empty, so catch() will be executed.
    console.log("catch");
}).finally(function() {
    console.log("finally");
});
/**
執行結果：
async
catch
finally
*/
```

