---
title:  "Firebase雲端資料庫 - 建立 Firebase 專案"
excerpt: "Firebase 是 Google 的雲端資料庫，在這個平台建立物聯網的專案，讓網頁與 IoT 設備能存取這個資料庫。"
header:
  teaser: assets/images/google/firebase-create-project.png
search: false
categories: 
  - Databases
tags:
  - Firebase
  - Google
  - 雲端資料庫
last_modified_at: 2020-04-25T21:00-00:00
toc: false
---
對於每天接觸資料庫的我來說，雲端資料庫的應用還是感到很好奇，想要使用 Firebase 就必需建立 Firebase 專案， Firebase 專案就像一個容器，可以容納你的 iOS 、 Android 和網路應用程式。

## 建立 Firebase 專案
首先進入 [Firebase Console](https://console.firebase.google.com/)，這裡就略過註冊帳號，假設你已經有登入 Google 帳號了，就跟著下列步驟建立專案。

Firebase 專案也就是 Cloud 專案，透過 Firebase 主控台建立 Firebase 專案時，你建立的其實是 Google Cloud Platform(GCP)專案[^gcp-project]。

[^gcp-project]: [Relationship between Firebase projects and Google Cloud Platform (GCP)](https://firebase.google.com/docs/projects/learn-more?authuser=0#firebase-cloud-relationship)

<figure class="align-center">
<img src="{{ site.url }}{{ site.baseurl }}/assets/images/google/firebase-create-project.png" alt="">
</figure> 
**Step 1**
1. 輸入專案名稱，名稱**只能使用字母、數字、空格和以下字元：-!'"**，若輸入了不合法的字元也會提示你，且不得少於 4 個字元，這裡的專案名稱算是對外顯示的別名。
2. 勾選接受 Firebase 條款。
3. 點選[繼續](#link){: .btn .btn--info}
4. 專案 ID (可略)，如果你輸入的專案 ID 在系統裡沒有重複，專案 ID 就會等於你的專案名稱，若是重複了，系統會自動在名稱之後加上編碼 ID ，若你不喜歡也可以去修改它，但有下列規則：
    * 專案 ID 的開頭必須是小寫字母，
    * 只能包含數字、字母和連字號(-)。
<figure class="align-center">
<a href="/assets/images/google/firebase-create-project-name-step1.png"><img src="{{ site.url }}{{ site.baseurl }}/assets/images/google/firebase-create-project-name-step1.png" alt=""></a>
</figure> 

**Step 2**
1. 啟用 Google Analytics (分析) 功能，如果沒有申請 GA 就停用，但建議是啟用這個服務。
2. 點擊[繼續](#link){: .btn .btn--info}
<figure class="align-center">
<a href="/assets/images/google/firebase-create-project-ga-step2.png"><img src="{{ site.url }}{{ site.baseurl }}/assets/images/google/firebase-create-project-ga-step2.png" alt=""></a>
</figure> 

**Step 3**
1. 選擇或建立 Google Analytics (分析) 帳戶，若沒有就在這裡新增即可，「帳戶」是 Analytics (分析) 的存取點，一個帳戶可以含有多項 Firebase 專案。[進一步瞭解帳戶](https://support.google.com/analytics/answer/1009618?ref_topic=3544906&authuser=0)。
2. 數據分析位置，這個欄位代表貴機構所在的國家/地區，這項設定不會影響 Google 在哪個位置處理及儲存 Firebase 的「客戶資料」。[瞭解詳情](https://firebase.google.com/support/guides/locations)
3. 勾選使用預設的 Google Analytics (分析) 資料共用設定。
4. 勾選接受評估服務控管者與控管者的資料保護條款與歐盟地區使用者同意授權政策。
5. 勾選接受 Google Analytics (分析) 的條款。
6. 點擊[建立專案](#link){: .btn .btn--info}
<figure class="align-center">
<a href="/assets/images/google/firebase-create-project-ga-step3.png"><img src="{{ site.url }}{{ site.baseurl }}/assets/images/google/firebase-create-project-ga-step3.png" alt=""></a>
</figure> 
當出現「新專案已準備就緒」就可以點擊[繼續](#link){: .btn .btn--info} 進入到下圖的專案總覽頁面，專案算是建立完成了。
<figure class="align-center">
<a href="/assets/images/google/firebase-project-overview.png"><img src="{{ site.url }}{{ site.baseurl }}/assets/images/google/firebase-project-overview.png" alt=""></a>
</figure>

建立好專案之後，可以看到左方的選單有許多 Firebase 產品與功能，後面我們就來介紹一下開發選單的 Database 與 Hosting這二個產品。

## 參考文章