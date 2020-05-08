---
title:  "Firebase雲端資料庫 - 樹莓派安裝Admin SDK"
excerpt: "Firebase 能夠提供毫秒級的回應，這樣樹莓派不但能夠把資料儲存這 Firebase 資料庫，同時不同的設備也能藉此溝通訊息，必要的條件就是安裝 Admin SDK了。"
header:
  teaser: assets/images/google/firebase-admin-sdk.png
search: false
categories: 
  - Databases
tags:
  - Firebase
  - Google
  - 雲端資料庫
  - 樹莓派
  - Python
last_modified_at: 2020-05-01T21:00-00:00
toc: true
---
<figure class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/google/firebase-admin-sdk.png" alt="">
</figure> 

Firebase 也提供了在 Server 安裝 Admin SDK[^admin_sdk] 就可以使用，同時提供多種語言的版本，前面我們介紹樹莓派裡使用 Python ，因此下面我們也是用 Python 來安裝 SDK。

[^admin_sdk]: [Add the Firebase Admin SDK to your server](https://firebase.google.com/docs/admin/setup?authuser=1)

## 前置準備
* [樹莓派安裝Raspbian作業系統（Windows篇）](/aiot/raspberry-raspbian-1-installation/)
* [樹莓派安裝 Microsort 遠端桌面](/aiot/raspberry-raspbian-2-installation-xrdp/)
* [在樹莓派建立Python虛擬環境](/aiot/raspberry-pip3-create-env/)
* [Firebase雲端資料庫 - 建立 Firebase 專案](/databases/firebase-create-project/)

## 教具準備

| 名稱 | 數量 | 規格 | 備註 | 
|:-------|:-----:|:----|:-----| 
| 樹莓派 | 1 | Raspberry Pi 4 Model B (4G) | 就只是現在頂規是這個版本，用三代的也可以 |

## 作業環境
* 樹莓派：Raspbian
* Windows 10 專業版 x64

## 安裝 SDK
當你順利進入樹莓派後，打開終端機 (Terminal)，進入你建立的虛擬環境，可以參考筆者前置準備的文章，筆者是建立了名稱為 dsalearning 的虛擬環境，進入的命令如下：
```bash
$ source ./bin/activate
```
筆者有固定一個目錄是放虛擬目錄，所以先進入到該虛擬環境的資料夾，進入成功後，會在最前方有 (虛擬環境名稱)，結果如下：
```
pi@raspberrypi:~ $ cd Documents/envs/
pi@raspberrypi:~/Documents/envs $ ls -al
total 12
drwxr-xr-x 3 pi pi 4096 Apr  2 20:24 .
drwxr-xr-x 4 pi pi 4096 Apr  4 22:16 ..
drwxr-xr-x 4 pi pi 4096 Apr  2 20:24 dsalearning
pi@raspberrypi:~/Documents/envs $ source dsalearning/bin/activate
(dsalearning) pi@raspberrypi:~/Documents/envs $ 
```
筆者有先列出 `dsalearning` 資料夾的權限，是我們現在進入樹莓派的帳號，不是 `root` ，如果你的資料夾權限是 `root` ，請你先修改權限給登入的帳號，以筆者的登入帳號是預設的 `pi` 帳號，這目的是不用 `sudo` 來安裝，那麼開始來安裝 Admin SDK，命令如下：
```bash
$ pip install firebase-admin
```
安裝會花一點點時間下載與安裝，當你看到 `Successfully built msgpack` 與 `Successfully installed` 就應該都安裝好了。

## 初始化 SDK

在樹莓派安裝好 SDK 後，表示我們的應用程式可以使用認證 Firebase 的帳戶調用其提供的 API ，這個就是你要先建好 Firebase 專案，並授權應用程式能訪問 Firebase 的雲服務，因此必須在你的專案產生私密金鑰，以這裡是樹莓派要使用的，所以要把這個私密金鑰上傳到樹莓派指定的路徑去，因為我們的應用程式會去參考這個私密金錀，接下來筆者會在 Windows 10 本機操作，產生私密金鑰的步驟如下。

1. 選取 Firebase 專案，以筆者的範例是 AIoTFarm 。
2. 在 Firebase 控制台點選 `設定(齒輪)` > `專案設定` > `服務帳戶` 。
<figure class="align-center">
  <a href="/assets/images/google/firebase-admin-sdk-step1.png"><img src="{{ site.url }}{{ site.baseurl }}/assets/images/google/firebase-admin-sdk-step1.png" alt=""></a>
</figure> 
3. 點選 `服務帳戶` 後，因為筆者是用 Python 開發，所以點選 Python，接著就點擊[產生新的私密金鑰](#link){: .btn .btn--info}，這時會提醒你**金鑰可用來存取專案的 Firebase 服務，因此請勿外洩，也不要儲存在公開的存放區**，請妥善保存這個檔案，金鑰一旦遺失即無法重新取得，點擊[產生金鑰](#link){: .btn .btn--info}就會下載一個 JSON 檔。

<figure class="half">
  <a href="/assets/images/google/firebase-admin-sdk-step2.png"><img src="{{ site.url }}{{ site.baseurl }}/assets/images/google/firebase-admin-sdk-step2.png" alt=""></a>
  <a href="/assets/images/google/firebase-admin-sdk-step2-1.png"><img src="{{ site.url }}{{ site.baseurl }}/assets/images/google/firebase-admin-sdk-step2-1.png" alt=""></a>
</figure> 

來看一下剛下載的 JSON 檔的內容。
<figure class="align-center">
  <a href="/assets/images/google/firebase-admin-sdk-key-json.png"><img src="{{ site.url }}{{ site.baseurl }}/assets/images/google/firebase-admin-sdk-key-json.png" alt=""></a>
</figure> 

由於筆者是在 Windows 10 操作的，並且是利用 Microsoft 的遠端桌面，所以就可以先遠端進樹莓派，直接複製金鑰 JSON 檔到樹莓派的指定位置，相信把金鑰複製到你指定的位置有很多方法，這裡不一定要按照筆者的方法去做，因為筆者的工作上也常使用這個工具，如果你也要使用這個工具，請先把下圖的這個設定開啟。

<img src="{{ '/assets/images/google/firebase-admin-sdk-xrdp.png' | relative_url }}" alt="Raspberry Pi Taskbar">

如果沒有先開啟也沒有關係，在你貼檔案到樹莓派時，系統會跳出下列的訊息，點擊[設定](#link){: .btn .btn--primary}後，會關閉遠端桌面程式，並開啟「檔案系統」的設定。

<img src="{{ '/assets/images/google/firebase-admin-sdk-xrdp1.png' | relative_url }}" alt="Raspberry Pi Taskbar">

筆者也順利的把金鑰複製到樹莓派了，下一篇再來介紹怎麼使用 SDK 。

<figure class="align-center">
  <a href="/assets/images/google/firebase-admin-sdk-key-json-pi.png"><img src="{{ site.url }}{{ site.baseurl }}/assets/images/google/firebase-admin-sdk-key-json-pi.png" alt=""></a>
</figure> 

## 參考文章