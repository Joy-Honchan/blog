---
title: Electron 概念介紹
date: 2023-07-05
tags: [Electron]
---

## What is Electron for?

Electron 簡單來說就是使用 Web 技術寫桌面應用程式，讓開發者不需要另外學新的語言。
這邊介紹幾個 Electron 的基本概念。

## Main Process & Render Process

Main 是主程序，Render 是單個視窗的程序。

Main 執行後會產生個別的 Render process，所以各視窗間的 render 不影響。

除了在 Main process 可以產生新 Render process 之外， Render process 也可以產生新 Render process (EX: 在 Menu 中設置 click={createWindow} )

## Preload

因為 render 不會執行 Node js, 只有 Main 會在 Node 環境中執行 所以要用 preload 來將 Node 中的功能引用過去。

使用 `**contextBridge.exposeInMainWorld**`( 變數 id, () ⇒ ({ 名稱: () ⇒ 回傳東西 }))

[使用预加载脚本 | Electron](https://www.electronjs.org/zh/docs/latest/tutorial/tutorial-preload)

## IPC

Inter-process communication 指的是 Main Process 與 Render Process 之間的溝通。electron 提供 **`ipcMain`** 作為 Main Process 端的 module，**`ipcRenderer`** 作為 render 端的 module。由於直接將 ipcRenderer 暴露於 render process 中會有安全疑慮，所以會在 Preload 裡創造限定 function 再用 `**contextBridge.exposeInMainWorld**` 送出。

[进程间通信 | Electron](https://www.electronjs.org/zh/docs/latest/tutorial/ipc)

### Render to Main 單向 (ipcRenderer.send ⇒ ipcMain.on)

將視窗收到的資料傳給 Node.js 做處理

**ipcRenderer.send( `channel`, `args`)** -從 render 發送 args，會被同 channel 的 ipcMain.on 接住

**ipcMain.on( `channel`, `(event, args) ⇒ {…}` )** -接收同 channel 的 ipcRenderer.send 的 event 和 args，event 為 ipcMainEvent，可從裡面取得發起 send 的視窗

```tsx
// preload.js
const { contextBridge, ipcRenderer } = require("electron");

const ipcFunc = { send: (data) => ipcRenderer.send("for_ipcMain", data) };
contextBridge.exposeInMainWorld("ipcFunc", ipcFunc);
```

```tsx
// main.js 官網放置在 createWindows 或 app.whenReady 裡
const { ipcMain } = require('electron');
...
  ipcMain.on('ipcR send', (event, data) => {
    console.log(data)
  })
```

```tsx
// index.tsx
declare global{ ipcFunc:{ send: (data)=>void } }
const handleSend = (e) => window.ipcFunc.send(e.target.value)
```

### Render to Main 雙向 (ipcRenderer.invoke ⇒ ipcMain.handle)

將視窗收到的資料傳給 Node.js 做處理，並再回傳給視窗

**ipcRenderer.invoke( `channel`, `args`)** -從 render 發送 args，會被同 channel 的 ipcMain.handle 接住，invoke 是一個 Promise。

**ipcMain.handle( `channel`, `(event, args) ⇒ {…}` )** -接收同 channel 的 ipcRenderer.send 的 event 和 args，event 為 ipcMainEvent，可從裡面取得發起 send 的視窗

```tsx
// preload.js
const { contextBridge, ipcRenderer } = require("electron");

const ipcFunc = { invoke: (data) => ipcRenderer.invoke("for_ipcMain", data) };
contextBridge.exposeInMainWorld("ipcFunc", ipcFunc);
```

```tsx
// main.js 官網放置在 createWindows 或 app.whenReady 裡
const { ipcMain } = require('electron');
...
  ipcMain.handle('ipcR send', (event, data) => {
    ...
		return "Done"
  })
```

```tsx
// index.tsx
declare global {
  ipcFunc: {
    invoke: (data) => Promise<string>;
  }
}
const handleSend = async (e) => {
  const result = await window.ipcFunc.send(e.target.value);
  console.log(result);
};
```
