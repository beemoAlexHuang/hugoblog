---
title: 網頁效能問題改善之 Debounce & Throttle
description: Welcome to Hugo Theme Stack
slug: 網頁效能問題改善之 Debounce & Throttle
date: 2022-08-03 00:00:00+0000
image: cover.jpg
categories:
    - 前端相關
tags:
    - 前端
---

## 適用情境

---

防止前端連點連續發送Request 影響效能

## Debounce

---

可在限定時間內存入Event 至 Event Queue ，指定時間內輸出所有存入Event Queue 的 Event

## Throttle

---

在指定的時間區間內只接受一次Evnet

## 核心組件

---

```jsx
(function(window) {
    var $ = window.jQuery || window.me || (window.me = {}),
        throttle = function(fn, timeout, callback, delayed, trailing, debounce) {
            timeout || (timeout = 100);
            var timer = false,
                lastCall = false,
                hasCallback = (typeof callback === "function"),
                startTimer = function(wrapper, args) {
                    timer = setTimeout(function(){
                        timer = false;
                        if (delayed || trailing) {
                            fn.apply(wrapper, args);
                            if (trailing) { lastCall = +new Date(); }
                        }
                        if (hasCallback) { callback.apply(wrapper, args); }
                    }, timeout);
                },
                wrapper = function(){
                    if (timer && !debounce) { return; }
                    if (!timer && !delayed) {
                        if (!trailing || (+new Date()-lastCall) > timeout) {
                            fn.apply(this, arguments);
                            if (trailing) { lastCall = +new Date(); }
                        }
                    }
                    if (debounce || !trailing) { clearTimeout(timer); }
                    startTimer(this, arguments);
                }
            if ($.guid) { wrapper.guid = fn.guid = fn.guid || $.guid++; }
            return wrapper;
        };
    $.throttle = throttle;
    $.debounce = function(fn, timeout, callback, delayed, trailing) {
        return throttle(fn, timeout, callback, delayed, trailing, true);
    };
})(this);
© 2022 GitHub, Inc.
```

## Demo

---

The most basic way to create a throttled function, pass it to $.throttle like this:

`var fn = $.throttle(fn, [timeout], [callback], [delayed], [trailing]);`

The same way goes when creating a debounced function:

`var fn = $.debounce(fn, [timeout], [callback], [delayed], [trailing]);`

```jsx
$("#searchbox").keyup($.debounce(function(){
    // Add your ajax code here
}, 250, null, true));
```

## 參考來源

---

[https://ithelp.ithome.com.tw/articles/10222749](https://ithelp.ithome.com.tw/articles/10222749)

[https://github.com/mekwall/jquery-throttle](https://github.com/mekwall/jquery-throttle)