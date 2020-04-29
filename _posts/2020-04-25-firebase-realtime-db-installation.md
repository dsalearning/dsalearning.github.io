---
title:  "Firebase雲端資料庫 - Realtime Database安裝"
excerpt: "Firebase 提供了 SDK 讓用戶端建構應用程式，能夠即時的存儲與同步資料，任何連接的設備都可以在幾毫秒內收到更新，這樣不同的設備也能彼此溝通，來看看怎麼安裝吧。"
header:
  teaser: assets/images/google/firebase-realtime-db.png
search: false
categories: 
  - Databases
tags:
  - Firebase
  - Google
  - 雲端資料庫
last_modified_at: 2020-04-28T21:00-00:00
toc: true
---
<figure class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/google/firebase-realtime-db.png" alt="">
</figure> 

Firebase [^firebase] 是 Google 的雲端資料庫，資料是以 JSON 格式儲存的 NoSQL 資料庫，當用戶端 (iOS, Android, Web, Window, ...) 使用 firebase SDK 建構應用程式時，能夠即時的存儲與同步資料，任何連接的設備都可以在幾毫秒內收到更新，這樣不同的設備也能彼此溝通，這就有很多的創意可以發想了。也支援離線 (非同步)的存取，先儲存在本地的磁碟，當設備重新連線之後就開始同步，自動合併所有的衝突，本篇介紹怎麼建立資料庫。

[^firebase]: [Firebase Realtime Database Introduction](https://firebase.google.com/docs/database?authuser=0)

## 前置準備
* [Firebase雲端資料庫 - 建立 Firebase 專案](/databases/firebase-create-project/)

## 建立資料庫
根據[前一篇](/databases/firebase-create-project/)已經有建立 Firebase 專案了，所以這裡就點選你的專案，進入到該專案總覽的頁面。

### Step1 建立 Realtime Database
1. 左方選單點擊 Database。
2. 主頁面往下滑到 Realtime Database，點擊[建立資料庫](#link){: .btn .btn--info}。

**注意！** Cloud Firestore 與 Realtime Database 是不同的產品，不要選錯了。
{: .notice--danger}

<figure class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/google/firebase-realtime-db-step1.png" alt="">
</figure> 

### Step2 以測試模式啟動
1. 選擇**以測試模式啟動**，擁有您資料庫參考資料的所有使用者都能讀取或寫入您的資料庫，聽起來似乎很有資安問題，別擔心，後面我們再來介紹安全性的設定。
2. 點擊[啟用](#link){: .btn .btn--info}
<figure class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/google/firebase-realtime-db-step2.png" alt="">
</figure> 

啟用之後，就會看到像下圖一樣，有出現一段警語，這是因為我們選擇了「測試模式」，安全性規則定義是公開的，所以可以忽略，而這時資料庫是已建立完成，可以著手設計了。

<figure class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/google/firebase-realtime-db-done.png" alt="">
</figure> 

Firebase 能夠支援 App 、 Web 、 前後端開發與物聯網的開發，你不用熟資料庫與 API 的開發就可以存取與使用，真是一個化繁為簡的產品，免費方案[^SparkPlan]有 1GB 空間、用量達 10GB/month ，對於入門的使用者來說已經足夠了。但要使用這個產品還是有些要先了解的，現在 Realtime Database 已建立好了，接下來就來看怎麼設計 [Realtime Database 的安全性規則](/databases/firebase-realtime-db-security/)吧。

[^SparkPlan]: [Firebase Pricing plans](https://firebase.google.com/pricing/)

## 參考文章