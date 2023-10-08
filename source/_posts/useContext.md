---
title: useContext 介紹
date: 2023-05-27
tags: [React, Hook, useContext]
---

根據 React 官方文件，useContext 可以解決 Prop Drilling 的問題，但 Prop Drilling 是什麼呢? Context 又是什麼?

<!--more-->

# 名詞解釋

## Prop Drilling

在 React 中，傳統傳遞資料的方法是由 Parent Component 傳 Prop 給 Child Component，但如果要傳到更深層的 Component 或是同一層的 Components 互傳，就會造成 Component 接收和傳遞自身不需要的 Prop，因為在樹狀圖看起來 Prop 將 Component 鑽穿，所以叫 Drill (鑽孔)。

## Context

Context 可以讓一個 Parent component 傳遞資料到旗下的所有 Component。
這個資料投射的概念叫 flux。做法是將資料放在一個 context 裡，哪個 Component 需要使用再 import 來使用。

# Context 可以解決什麼?

1. Prop 的 Source Component 離 Receive Component 太遠
2. 相同的 Prop 要同時傳遞給多個 Component

## 其他解決方案

1. 如果不用傳太多層，傳送 Prop 還是較佳的方法
2. 試著抽出元件並將 JSX 當作 Prop

## 使用情境

每個 Component 都需要知道的資料

1. Theme：如 Dark mode
2. User info：如：登入狀態、使用者身分不同會 render 不同 Component 等

資料本身及改變他的 function 都可放入 context 中

# useContext 使用方式

## createContext( initialValue )

創建一個 context，可以把它想像成一筆資料

```javascript
//通常會獨立建立一個檔案
// LevelContext.js
export const LevelContext = createContext(0);
// 如同 const [ data, setData ] = useState(null) 裡的 data
```

## Context.Provider

用 Context.Provider 設定此 context 的影響範圍及用 value 屬性提供值

```javascript
//Source Component
import { LevelContext } from './LevelContext.js'
...
return (
    <div className="section">
      <LevelContext.Provider value={5}>
				{children}
			</LevelContext.Provider>
    </div>
  );
```

## useContext

用 useContext 指定要收哪一個 Context 的資料

```javascript
//Receive Component
import { useContext} from 'react'
import { LevelContext } from './LevelContext.js'
...
const dataFromContext = useContext(LevelContext)
```

> 💡 甚至可以將從上層 Context 接收到的資料加工後再傳下去，因為 useContext 會去找距離最近的 Provider，return 該值。
> 當 return 回來的值改變，就會觸發 re-render

```javascript
import { useContext } from "react";
import { LevelContext } from "./LevelContext";
...
  const level: number = useContext(LevelContext);
  ...
  return (
    <div className="section">
      <LevelContext.Provider value={level + 1}>
        {children}
      </LevelContext.Provider>
    </div>
  );
```
