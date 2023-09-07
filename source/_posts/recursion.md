---
title: Recursion 遞迴
date: 2023-03-13
tags: [JavaScript, Recursion]
---

# 什麼是遞迴(**_Recursion_**)?

簡單來說，遞迴就是一個自己呼叫自己的函式。
為了避免無窮迴圈，在 Recursion 中必須要有一個出口機制(**_Base Case_**)。可做為迭代(**_iteration_**)，如 for 、 while loop 的替代，但通常效率及記憶體使用量都較差。

<!--more-->

## 題目範例

```
請寫出一函式 countDown，放入數字參數 n 後執行會照順序 console 出 Only n left, Only n-1 left... Only 2 left, Only 1 left, Done.
```

```javascript
// iteration Example
function countDown(n) {
  for (let i = n; i > 0; i--) {
    console.log(`Only ${i} left`);
  }
  console.log("Done!");
}

// Recursion Example
function countDownRecursive(n) {
  // 先設定出口
  if (n <= 0) {
    console.log("Done!");
    return;
  }
  console.log(`Only ${n} left`);
  countDownRecursive(n - 1);
}
```
