---
title: 三種 For Loop 一次學
date: 2023-08-09
tags: [For Loop, JavaScript]
---

通常初學者學到 for loop，應該都是從以下陳述式開始學

> for( 起點; 終點; 如何遞增 ){ ... }

```javascript
const arr = ["Ben", "Ken", "Men"];
for (let i = 0; i < arr.length; i++) {
  console.log(`${arr[i]} is here!`);
}
// "Ben is here!"
// "Ken is here!"
// "Men is here!"
```

但 for loop 還有另外兩種

1. for ( **_item_** of **_iteration_** ) loop value，只能用在可 iterate 的物件(Array, String, Map)，所以一般 Object 不可用

### Array

```javascript
const arr = ["Ben", "Ken", "Men"];
for (const name of arr) {
  console.log(`${name} is here!`);
}
// "Ben is here!"
// "Ken is here!"
// "Men is here!"
```

### String

```javascript
const str = "ab c";
for (const letter of str) {
  console.log(`It's ${letter}`);
}
// "It's a"
// "It's b"
// "It's  "
// "It's c"
```

2. for( **_item_** in **_iteration_** ) loop key，
   用在有 key 的，如 Array 和 Object

### Object

```javascript
const arr = { Ben: "teacher", Ken: "student", Men: "parent" };
for (const name in arr) {
  console.log(`${name} is ${arr[name]}!`);
}
// "Ben is teacher!"
// "Ken is student!"
// "Men is parent!"
```

### Array

```javascript
const arr = ["a", "b", "c"];
for (const index in arr) {
  console.log(`${arr[index]} is No.${index}!`);
}
// "a is No.0!"
// "b is No.1!"
// "c is No.2!"
```
