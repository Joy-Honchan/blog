---
title: 為什麼不用 Index 做 React List Item 的 key?
date: 2023-04-11
tags: [React]
---

“If you don’t add key to each mapped element, React will b\*tch about it” 這是我在某個 tutorial 裡聽到的。因為只要將 index 加上就可以避免報錯，我也就沒有太在意到底 key 的用意為何。

當 React 在用 VirtualDom 作比對的時候，他要有辦法辨識哪些 element 是新的、哪些 element 有做改變。特別是 map 出來的 element，因為 render 出來的東西都一樣只是裡面的參數不同，所以必須要有獨特的 key 讓 React 去做比對。

<!--more-->

當 List Element 是向下增長時，因為 index 不會重新分配，較不會出現問題。但當 List Element 會重新排序( EX: 從上面新增 or sort )時，React 會沒有依據去分辨 element 在舊 VirtualDom 中是否變動或存在，進而造成不必要的 re-render。當你的 DataList 是往下增長時，因為 index 累加而不是重新排序，較不會出現問題。

### 實作

{% iframe https://codesandbox.io/embed/why-key-in-react-list-05kmfb?fontsize=14&hidenavigation=1&theme=dark  %}

<!-- [React App](https://05kmfb.csb.app/) -->

1. ❌ 不加 key 的情況 - 因為無法辨認，每個 element 都會 re-render
2. ⚠️ 用 index 或 name+index 的情況 - 如果資料是從上面堆疊導致 index 重新排序，會造成沒有變化的 element 也被 re-render
3. ❌ 用亂數產生器 的情況 - 每次 render 都會被賦予新的 key，結果同 1.
4. ✔️ 用 list item 本身的 id，或在 addItem 裡的新增 unique key -

```jsx
addItem( originData ){
		const addKeyData = { ...originData, uniqueKey: new Date() }
		setDataList( prev => ([ addKeyData, …prev ]) );
}
```
