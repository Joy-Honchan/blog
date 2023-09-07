---
title: Closure 用途
date: 2023-03-17
tags: [JavaScript, Closure]
---

今天來聊聊 閉包 Closure 在幹嘛以及能幹嘛

巢狀 function 中，內部 function 仍可以外部 function 執行完畢後，取得外部 function 的變數

閉包的好處：

1. 保護變數不被其他函數所用
2. memory release 的時機點好掌握

```javascript
//EX: 每次按按鈕都會 console 出被按次數
btn.addEventListener("click", clickfire);

// num 露在外面，要整個 script 跑完 or 頁面關閉才會 release memory
let num = 0;

function clickfire() {
  num += 1;
  console.log(`I have been clicked ${num} times`);
}
```

```javascript
//使用閉包
const callbackFun = clickfire();
btn.addEventListener("click", callbackFun);

function clickfire() {
  // num 放在裡面，只有 callbackFun 動得了，且當 btn 的 element 從頁面消失，num 就會被 release
  let num = 0;

  return () => {
    num += 1;
    console.log(`I have been clicked ${num} times`);
  };
}
```
