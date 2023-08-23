---
title: CORS 跨域資源共享
date: 2023-03-22
tags: [CORS]
---

當網頁向 API 要求資料時，如果網頁的 URL 與 API URL 不相同時，瀏覽器會報錯

## Why

因為**瀏覽器**大多有同源政策安全機制，讓網頁可自由向自己的 URL 送出請求，但對其他 URL 的請求會被拒絕。

```
OK=> 在 cat.com 的網頁向 cat.com/api/users 發送請求
NG=> 在 cat.com 的網頁向 dog.com/api/users 發送請求
```

如果沒有同源政策可能會發生的情況：不肖網站在使用者不知情的情況下向銀行網站發送轉帳請求。

## How

瀏覽器會阻擋非同源的 Response。發送請求時，Request Header 自帶 **Origin** 屬性。

{% asset_img cors-origin.png %}

Server 回傳時，須設定帶 **Access-Control-Allow-Origin** 屬性。

{% asset_img cors-access-control-allow-origin.png %}

瀏覽器會對比以上兩個屬性，決定是否讓請求通過。

所以 CORS 問題只能從 Server 端解決。

## Preflight 機制

因為 PUT、POST、PATCH、DELETE 相較於 GET 風險大，所以在正式請求之前會有一次 Preflight。

```
📝 向 myurl.com/data 送出 PUT request
```

| Name  | Status           | Type      |
| ----- | ---------------- | --------- |
| /data | 204 (no content) | preflight |
| /data | 200              | xhr       |

在真正的 Request 之前 會先送出一個 **Request Method: OPTIONS** 的請求。

Response Header 會包括 **Access-Control-Allow-Method:** 可通過的 request 方法。

Request Header 會包括 **Access-Control-Request-Method:** 準備要執行的 request 方法。

此次 request 回報正確後才會發送真正的 request。

{% asset_img cors-preflight.png %}

如果要短時間內多次執行 PUT 或 POST，可以設定 preflight 結果的快取=> **Access-Control-Max-Age**

## CORS in Express.js

以 Express 為例，可使用 cors 套件增加請求的白名單。

```jsx
// 先安裝 CORS 套件 - npm install cors
const express = require('express');
const app = express();
const cors = require('cors');

app.use(
	cors({
		origin: 'http://mywebsite.com',
		// origin: *, //讓所有 url 都可通行
		methods: ['GET','PUT'] //限制此 origin 只能 GET PUT
		credentials: true // 是否傳送 cookie， 預設是 false
		// 前端發送的請求也必須包含 credentials: true
		// EX: axios.post('data',{ credentials:true })
	})
)

app.get('/data', (req,res)=>{
	res.json({ "name": "John" })
})

app.listen(3000)
```

來源：

[CORS in 100 Seconds](https://youtu.be/4KHiSt0oLJ0)

[Learn CORS In 6 Minutes](https://youtu.be/PNtFSVU-YTI)

[簡單弄懂同源政策 (Same Origin Policy) 與跨網域 (CORS)](https://medium.com/starbugs/%E5%BC%84%E6%87%82%E5%90%8C%E6%BA%90%E6%94%BF%E7%AD%96-same-origin-policy-%E8%88%87%E8%B7%A8%E7%B6%B2%E5%9F%9F-cors-e2e5c1a53a19)
