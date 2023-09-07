---
title: next@canary 與 map 時 key 的問題
date: 2023-08-15
tags: [Next, Error Shooting]
---

今天用 Next 開發時遇到一個狀況，在使用 MUI 的 [AutoComplete](https://mui.com/material-ui/react-autocomplete/) 時跳出兩個 Error，雖然沒有影響操作，但還是查了一下如何解決。

<!--more-->

## Warning: Prop htmlFor did not match ...

錯誤訊息還給了我 Server id 和 Client id，兩個 id 並不相同。我後來在一個 label 上找到 Server id，但仍不知道該如何解決。後來我在某個 UI 框架的 Github issue 裡找到[答案](https://github.com/shadcn-ui/ui/issues/1023#issuecomment-1657288374)。

解決方式是安裝 next@canary。Canary(金絲雀)可被視為測試開發版，他雖不如正式版穩定，但他的更新更頻繁。所以想要發 issue 請對方修正 bug 前，可以先跑跑看 Canary，因為可能已經修正只是未發布在正式版。

## Warning: A props object containing a "key" prop is being spread into JSX: ...

這次錯誤訊息直接點出問題所在: **_React keys must be passed directly to JSX without using spread_** 。會有 key 的問題通常是因為需要 map，而 AutoComplete 裡會跟 map 相關的就是選項的 list 了。應該是 MUI 會將使用者提供的選項 props 中加入 key，然後直接 props spread 在 element 上。但在用 Next 之前 AutoComplete 都沒有這樣的錯誤，所以應該是在 Next 中，key 的使用方式變嚴格了。

```javascript
// Wrong
let props = { key: someKey, label: ..., size: ..., className: ..., disabled: ...};
  <li {...props} />

// Correct
let props = { label: ..., size: ..., className: ..., disabled: ...};
  <li key={someKey} {...props} />
```

而 AutoComplete 中有提供 renderOptions 屬性，讓我們可以控制選項的 render，就可以利用這個屬性加入 key。

```javascript
<Autocomplete
  options={optionsData}
  renderOption={(props, option) => {
    return (
      <li {...props} key={option.id}>
        {option.name}
      </li>
    );
  }}
/>
```

[這個 stackoverflow 解答更完整](https://stackoverflow.com/questions/75818761/material-ui-autocomplete-warning-a-props-object-containing-a-key-prop-is-be)
