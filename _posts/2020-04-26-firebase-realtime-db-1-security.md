---
title:  "Firebase雲端資料庫 - 初探 Realtime Database 安全性規則"
permalink: /firebase/realtime-db-1-security/
excerpt: "Firebase Realtime Database 是透過 Rules (安全性規則)來讓資料多一層保護，規則除了可以設定欄位的讀取或寫入，也可以規範授權(auth)的使用者。"
header:
  teaser: assets/images/google/firebase-realtime-db-security.png
search: false
categories: 
  - 樹莓派
tags:
  - Firebase
  - Google
  - 雲端資料庫
  - Database
  - 待寫
sidebar:
  nav: "raspberry"
author_profile: false
last_modified_at: 2020-05-01T15:00-00:00
toc: true
---
<figure class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/google/firebase-realtime-db-security.png" alt="">
</figure> 

前一篇提到 Firebase 的資料是以 JSON 格式儲存的 NoSQL 資料庫，如果你有接觸過 MongoDB 的話，那你對這樣的資料格式一定很熟悉，同樣都是 NoSQL的資料庫，以JSON格式儲存，至於如何才能設計出好的資料庫，這個應該是需要經驗的累積，而資料的安全性也十分重要，它不需要經驗累積，只要熟悉該產品的安全性規則就可以限制了，由於前一篇建立的資料庫預設是公開的，所以接下來看怎麼去限制權限來保護我們的資料吧。

## 前置準備
* [Firebase雲端資料庫 - 建立 Firebase 專案](/databases/firebase-create-project/)
* [Firebase雲端資料庫 - Realtime Database安裝](/databases/firebase-realtime-db-installation/)

## Rule Types

Realtime Database Rules[^db_security] 具有類似 JavaScript 的語法，定義了下列四種屬性

| 屬性 | 型別 | 說明 |
|:-------|:--------:|:--------|
| `.read` |	boolean | 是否允許**讀取**資料， `true` 可以讀取；`false` 拒絕讀取。 |
| `.write` | boolean | 是否允許**寫入**資料， `true` 可以寫入；`false` 拒絕寫入。 |
| `.validate` | string	| 定義正確格式化後的值是什麼樣子，是否有子屬性、資料的類型等。 |
| `.indexOn` | string	| 指定索引來支持排序和查詢，在資料量大的時候才會感覺效能的差異。 |

[^db_security]: [Understand Firebase Realtime Database Rules](https://firebase.google.com/docs/database/security?authuser=0)

## 授權方式

以筆者接觸資料庫多年的經驗，資料的安全性是以最小化權限授權為原則，所以這裡也是一樣，授權給不同的用戶擁有不同的資料權限，同時資料也可以讓多人擁有相同的權限，前一篇我們建立**以測試模式啟動**的資料庫，其安全性是公開的，安全性規則的 JSON 如下：
```json
{
  "rules": {
    ".read": true,
    ".write": true
  }
}
```
它的 `.read` 和 `.write` 都是 `true` ，因此只要知道此資料庫位置的任何人都可以讀取與修改資料，這風險還真大啊！ Firebase 也很貼心的警示這樣的設計很危險，如下圖。

<figure class="align-center">
  <a href="/assets/images/google/firebase-realtime-db-security-rules-public.png"><img src="{{ site.url }}{{ site.baseurl }}/assets/images/google/firebase-realtime-db-security-rules-public.png" alt=""></a>
</figure> 

我們可以點擊[規則模擬工具](#link){: .btn .btn--info}來模擬看看。
1. 模擬類型： read
2. 已驗證：關閉
3. 點擊[執行](#link){: .btn .btn--info}
4. 得到此結果**已允許模擬「read」**，點擊詳細資料可以看到**驗證的值是 null**，讀取成功，因為 `.read` 的屬性值是 `true`。同理，把屬性值都設定為 `false` 時，所有人都無法讀取與修改了。

<figure class="half">
  <a href="/assets/images/google/firebase-realtime-db-security-rules-valid1.png"><img src="{{ site.url }}{{ site.baseurl }}/assets/images/google/firebase-realtime-db-security-rules-valid1.png" alt=""></a>
  <a href="/assets/images/google/firebase-realtime-db-security-rules-valid2.png"><img src="{{ site.url }}{{ site.baseurl }}/assets/images/google/firebase-realtime-db-security-rules-valid2.png" alt=""></a>
  <a href="/assets/images/google/firebase-realtime-db-security-rules-valid3.png"><img src="{{ site.url }}{{ site.baseurl }}/assets/images/google/firebase-realtime-db-security-rules-valid3.png" alt=""></a>
  <a href="/assets/images/google/firebase-realtime-db-security-rules-valid4.png"><img src="{{ site.url }}{{ site.baseurl }}/assets/images/google/firebase-realtime-db-security-rules-valid4.png" alt=""></a>  
</figure> 

那安全性規則要怎麼設計才能做到保護的作用？這個就取決於需求了，下面我們舉幾個例子吧！

### 只授權有認證過的使用者
首先先來設計資料如下圖
<figure class="align-center">
  <a href="/assets/images/google/firebase-realtime-db-security-data2.png"><img src="{{ site.url }}{{ site.baseurl }}/assets/images/google/firebase-realtime-db-security-data2.png" alt=""></a>
</figure> 

接下來到規則設計 JSON 為「只要有認證過的使用者都可以讀取與修改資料」，這個的權限其實開很大，陌生的人登入就可以進行讀取與修改，意思是只要註冊為用戶就可以，安全性規則的 JSON 如下：
```json
{
  "rules": {
    ".read": "auth != null",
    ".write": "auth != null"
  }
}
```
當你確認發佈後， Firebase 一樣很貼心的警示你這樣的設計不夠完善，如下圖。

<figure class="align-center">
  <a href="/assets/images/google/firebase-realtime-db-security-rules-notnull.png"><img src="{{ site.url }}{{ site.baseurl }}/assets/images/google/firebase-realtime-db-security-rules-notnull.png" alt=""></a>
</figure> 

我們可以點擊[規則模擬工具](#link){: .btn .btn--info}來模擬看看。
1. 模擬類型： read
2. 已驗證：關閉
3. 點擊[執行](#link){: .btn .btn--info}
4. 得到此結果**已拒絕模擬「read」**，點擊詳細資料可以看到**驗證的值是 null**，已拒絕讀取，因為 `.read` 的屬性值是 `auth != null`，意思就是要登入後才可以讀取。

<figure class="half">
  <a href="/assets/images/google/firebase-realtime-db-security-rules-valid21.png"><img src="{{ site.url }}{{ site.baseurl }}/assets/images/google/firebase-realtime-db-security-rules-valid21.png" alt=""></a>
  <a href="/assets/images/google/firebase-realtime-db-security-rules-valid22.png"><img src="{{ site.url }}{{ site.baseurl }}/assets/images/google/firebase-realtime-db-security-rules-valid22.png" alt=""></a>
</figure> 

接下來測試已驗證的結果，這次來測試 set 修改資料。
1. 模擬類型： set
2. 位置： /Demo ，就是前面我們設計的資料，各位會發現在 Demo 應該有上一層的 aiotfarm，但規則設定的是最上層，整個資料庫！所以可以了解規則是含蓋了更深層的路徑。
3. 資料： 將 Demo 節點下的 `status` 屬性設定為 `true`，這裡只要屬性值有值即可，只是測試可不可以寫入。
```json
{
  "status": "true"
}
```
4. 已驗證：開啟。
5. 供應商：都可以，因為驗證的條件不含這個。
6. UID：只要有值就可以，但必須先登入進來有認證過才可。
7. 點擊[執行](#link){: .btn .btn--info}
8. 得到此結果**已允許模擬「set」**，點擊詳細資料可以看到**驗證的值是有值**，結果是「寫入成功」，因為 `.write` 的屬性值是 `auth != null`，意思就是要登入後才可以寫入。

<figure class="half">
  <a href="/assets/images/google/firebase-realtime-db-security-rules-valid23.png"><img src="{{ site.url }}{{ site.baseurl }}/assets/images/google/firebase-realtime-db-security-rules-valid23.png" alt=""></a>
  <a href="/assets/images/google/firebase-realtime-db-security-rules-valid24.png"><img src="{{ site.url }}{{ site.baseurl }}/assets/images/google/firebase-realtime-db-security-rules-valid24.png" alt=""></a>  
</figure> 
不過這種的設計就如警示一樣「**不夠完善**」，任何通過驗證的使用者都能竊取、修改或刪除資料庫中的資料。

### 特定授權
即然 `auth != null` 的設定方式不夠完善，那要怎麼設定才完善？筆者還是認為專案需求決定最小化權限的設定，那現在我們就來設計相對安全的安性性規則，先來看看它的 JSON 如何設計。

```json
{
  "rules": {
    ".read": false,
    ".write": false,
    "aiotfarm": {
      "Demo":{
        "$uid": {
        ".read": "$uid === auth.uid",
        ".write": "$uid === auth.uid"
      	}
      }
    }
  }
}
```
這個不難理解，就是把所有的使用者預設都是不能讀取與寫入，但授權 `/aiotfarm/Demo/` 這個路徑允許認證的使用者讀取與寫入該路徑的資料， `$uid` 是系統的變數，當通過 Firebase 身份驗證後就會得到的用戶 ID 。

在測試之前，我們先看一下這次的資料設計，如下圖。
<figure class="align-center">
  <a href="/assets/images/google/firebase-realtime-db-security-data3.png"><img src="{{ site.url }}{{ site.baseurl }}/assets/images/google/firebase-realtime-db-security-data3.png" alt=""></a>
</figure> 

在 `aiotfarm` 主節點下新增了 `Demo` 與 `devices` 這二個子節點，接下來的測試是登入的使用者有 `Demo` 路徑的權限，但沒有 `devices` 的權限，接下來測試讀取已驗證的結果。
1. 模擬類型： read
2. 位置： /aiotfarm/devices/$uid ，當通過 Firebase 身份驗證後就會得到的用戶 ID ，筆者這裡遮蔽了自己的 ID ，各位請帶上自己的吧，可以從 #5 得知。
3. 已驗證：開啟。
4. 供應商：預設帶入的 Google 就可以。
5. UID：當通過 Firebase 身份驗證後就會得到的用戶 ID ，把這組 ID 加入到位置的路徑。
6. 點擊[執行](#link){: .btn .btn--info}
7. 得到此結果**已拒絕模擬「read」**，點擊詳細資料可以看到**驗證的值是有 uid**，結果是「已拒絕讀取」，因為預設根目錄 `.read` 的屬性值是 `false`，且並沒有指定該位置的路徑是可以讀取。

<figure class="half">
  <a href="/assets/images/google/firebase-realtime-db-security-rules-valid31.png"><img src="{{ site.url }}{{ site.baseurl }}/assets/images/google/firebase-realtime-db-security-rules-valid31.png" alt=""></a>
  <a href="/assets/images/google/firebase-realtime-db-security-rules-valid32.png"><img src="{{ site.url }}{{ site.baseurl }}/assets/images/google/firebase-realtime-db-security-rules-valid32.png" alt=""></a>
</figure> 

那剛才的安全性規則有設定了 `/aiotfarm/Demo/` 這個路徑是可以讀寫，接著測試看看吧！
1. 模擬類型： read
2. 位置： /aiotfarm/Demo/$uid ，這裡請特別**留意大小寫**是有差異的。
3. 已驗證：開啟。
4. 供應商：預設帶入的 Google 就可以。
5. UID：當通過 Firebase 身份驗證後就會得到的用戶 ID ，把這組 ID 加入到位置的路徑。
6. 點擊[執行](#link){: .btn .btn--info}
7. 得到此結果**已允許模擬「read」**，點擊詳細資料可以看到**驗證的值是有 uid**，結果是「讀取成功」，雖然預設根目錄 `.read` 的屬性值是 `false`，但下方有指定該位置的路徑是可以讀取。

<figure class="half">
  <a href="/assets/images/google/firebase-realtime-db-security-rules-valid33.png"><img src="{{ site.url }}{{ site.baseurl }}/assets/images/google/firebase-realtime-db-security-rules-valid33.png" alt=""></a>
  <a href="/assets/images/google/firebase-realtime-db-security-rules-valid34.png"><img src="{{ site.url }}{{ site.baseurl }}/assets/images/google/firebase-realtime-db-security-rules-valid34.png" alt=""></a>
</figure> 

### 定義規則順序

Firebase 的規則採用「**淺層判斷至深層**」[^security_sort]的做法，只要是存取資料，規則的 true 與 false 一律由淺層 ( root 根節點 ) 開始判斷起，以剛才的例子來說明，調整安全性規則的 JSON 語法如下：

[^security_sort]: [定義規則順序](https://www.oxxostudio.tw/articles/201904/firebase-realtime-database-rules.html)

```json
{
  "rules": {
    "aiotfarm": {
      "Demo":{
        "$uid": {
        ".read": "$uid === auth.uid",
        ".write": false
      	}
      }
    },
    ".read": true,
    ".write": true
  }
}
```
可以看到在根目錄就已設定為可以讀取跟寫入了，這樣另外在子節點再設定拒絕就變得多此一舉，我們來看看實際測試的結果吧。

<figure class="half">
  <a href="/assets/images/google/firebase-realtime-db-security-rules-valid41.png"><img src="{{ site.url }}{{ site.baseurl }}/assets/images/google/firebase-realtime-db-security-rules-valid41.png" alt=""></a>
  <a href="/assets/images/google/firebase-realtime-db-security-rules-valid42.png"><img src="{{ site.url }}{{ site.baseurl }}/assets/images/google/firebase-realtime-db-security-rules-valid42.png" alt=""></a>
</figure> 

從上圖可以看到，根本沒有走到子節點判斷，如果要針對節點設定權限，可以把根目錄的這個移除，這樣如果不寫讀寫的規則，會變成預設值 `false` ， `false` 不會影響深層，安全性規則的 JSON 語法如下：

```json
{
  "rules": {
    "aiotfarm": {
      "Demo":{
        "$uid": {
        ".read": "$uid === auth.uid",
        ".write": "$uid === auth.uid"
      	}
      }
    }
  }
}
```

### 資料驗證

不過**淺層判斷至深層**只適用於 `.read` 與 `.write` ， 另一個屬性 `.validate` 則不受這個規則的限制，只會影響自己的層級，例如上述案例的資料中，在 `/aiotfarm/Demo/` 下有個 `status` 屬性，資料的型別為布林值 (Boolean)，只接受 `true` 或 `false` ，因此我們可以用 `.validate` 來限制，安全性規則的 JSON 語法如下：。

```json
{
  "rules": {
    "aiotfarm": {
      "Demo":{
        "$uid": {
        ".read": "$uid === auth.uid",
        ".write": "$uid === auth.uid",
        ".validate": "newData.child('status').isBoolean()"
      	}
      }
    }
  }
}
```

接著來測試寫入的結果吧。
1. 模擬類型： set
2. 位置： /aiotfarm/Demo/$uid ，這裡請特別**留意大小寫**是有差異的。
3. 資料：注意屬性值只有 `true` 或 `false` 。
```json
{
  "status": false
}
```
4. 已驗證：開啟。
5. 供應商：預設帶入的 Google 就可以。
6. UID：當通過 Firebase 身份驗證後就會得到的用戶 ID ，把這組 ID 加入到位置的路徑。
7. 點擊[執行](#link){: .btn .btn--info}
8. 得到此結果**已允許模擬「set」**，點擊詳細資料可以看到**驗證的值是有 uid**，結果是「寫入成功」，可以看到 `.validate` 驗證是成功的。

<figure class="half">
  <a href="/assets/images/google/firebase-realtime-db-security-rules-valid43.png"><img src="{{ site.url }}{{ site.baseurl }}/assets/images/google/firebase-realtime-db-security-rules-valid43.png" alt=""></a>
  <a href="/assets/images/google/firebase-realtime-db-security-rules-valid44.png"><img src="{{ site.url }}{{ site.baseurl }}/assets/images/google/firebase-realtime-db-security-rules-valid44.png" alt=""></a>
</figure> 

那麼來測試失敗，把 `status` 屬性值改為字串的 `"false"`，結果如下。

<figure class="align-center">
  <a href="/assets/images/google/firebase-realtime-db-security-rules-valid45.png"><img src="{{ site.url }}{{ site.baseurl }}/assets/images/google/firebase-realtime-db-security-rules-valid45.png" alt=""></a>
</figure> 

### 定義資料庫索引
待寫。

## 參考文章