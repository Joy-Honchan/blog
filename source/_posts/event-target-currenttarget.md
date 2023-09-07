---
title: event.target，currentTarget 傻傻分不清楚?
date: 2023-09-08
tags: [JavaScript, Event Target]
---

這是我在初學 TypeScript 時遇到的問題，現在做個補充筆記。

React Form 的 onSubmit ，callback 裡的 event.target 總是找不到 element 屬性
改成 currentTarget 即可 - **e.currentTarget .sound.value**

<!--more-->

```typescript
// Error: Property 'sound' does not exist on type 'EventTarget'
const handleSubmit: React.FormEventHandler<HTMLFormElement> = (e) => {
  e.preventDefault();
  console.log(e.target.sound.value);
};

return (
  <form onSubmit={handleSubmit}>
    <input name="sound" />
    <button type="submit">
      <span role="img" aria-label="music">
        🎵
      </span>
    </button>
  </form>
);
```

## event.target

- 點下去觸發的物件
- Type : EventTarget

## event.currentTarget

- addEventListener 的物件
- Type：EventTarget & addEventListener 的物件的 Type
  EX: Form-HTMLFormElement、button-HTMLButtonElement

```typescript
const btnClick: React.MouseEventHandler<HTMLButtonElement> = (e) => {
  // 點擊在圖案上
  console.log(e.target); //<span role="img" aria-label="quiet">🤫</span>
  console.log(e.currentTarget);
  //<button type="button">
  //  <span role="img" aria-label="quiet">🤫</span>
  //</button>
};

return (
  <button type="button" onClick={btnClick}>
    <span role="img" aria-label="quiet">
      🤫
    </span>
  </button>
);
```

## 不使用 Form 的情況下

```typescript
const [page, setPage] = useState(1);

function handlePage(e: React.ChangeEvent<HTMLSelectElement>) {
  // console.log(e.target.value);
  setPage(parseFloat(e.target.value));
}

// onChange => (property) React.SelectHTMLAttributes<HTMLSelectElement>.onChange?:
// React.ChangeEventHandler<HTMLSelectElement> | undefined

<select value={page} onChange={handlePage}>
  <option value={1}>1</option>
  <option value={2}>2</option>
  <option value={3}>3</option>
</select>;
```
