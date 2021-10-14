---
title:  "樹莓派安裝Raspbian作業系統（Windows篇）"
permalink: /raspberry/1-installation/
excerpt: "Raspberry Pi 4 出來了，和之前的版本最大的改變是多了 2GB/4GB 的記憶體版本可供選擇！"
header:
  teaser: assets/images/raspberrypi/raspberry-pi-4-model-b.jpg
search: false
categories: 
  - 樹莓派
tags:
  - IoT
  - Raspbian
  - 物聯網
  - 安裝
sidebar:
  nav: "raspberry"
author_profile: false
last_modified_at: 2020-03-24T21:00-00:00
toc: true
---

<figure class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/raspberrypi/raspberry-pi-4-model-b.jpg" alt="">
  <figcaption><a href="https://www.raspberrypi.org/blog/raspberry-pi-4-on-sale-now-from-35/" title="Raspberry Pi 4 on sale now from $35">圖片來源：Raspberry Pi 4 on sale now from $35</a>.</figcaption>
</figure> 

## 前置準備
* [Raspberry Pi 4 Model B](https://www.raspberrypi.com.tw/28040/raspberry-pi-4-model-b/) ： 和之前的版本最大的改變是多了 2GB/4GB 的記憶體版本可供選擇，本次使用的是 4GB 版。
* 記憶卡 (SD Card) : 建議容量 16GB 以上[^sd-cards]，速度等級建議 Class 10。系統安裝最小建議是 8GB ，但安裝完系統後僅剩 1GB ，想再安裝其它軟體就很困難了。
* 讀卡機 : 準備利用電腦把樹莓派映像檔燒錄到 SD Card。
* 電源接頭(Power) : 使用 USB-C，使用 5V/3A 以上電源供應器才能穩定使用。
* 視訊接頭(Video) :  type-D (micro) HDMI 接頭，也就是 micro HDMI to HDMI cable。
* 螢幕 : 利用上述的視訊接頭，把樹莓派的系統影像輸出到螢幕，這樣比較方便設定。僅安裝後的第一次需要，之後就利用遠端桌面的工具登入操作。

[^sd-cards]: [the SD card requirements](https://www.raspberrypi.org/documentation/installation/sd-cards.md).


## 作業環境
* Windows 10 專業版 x64

### 安裝Raspbian(方法一)

一切都準備好了，開始來安裝[^installation]吧。

[^installation]: [installation guide](https://www.raspberrypi.org/documentation/installation/installing-images/README.md).

#### 1. 下載樹莓派映像檔
至[官網](https://www.raspberrypi.org/downloads/)下載最新的樹莓派映像檔（Raspberry Pi Imager)並安裝它。
<figure>
  <img src="{{ '/assets/images/raspberrypi/raspberry-pi-imager-download.png' | relative_url }}" alt="Raspberry Pi Imager">
</figure>

#### 2. 執行樹莓派映像檔
* 打開剛安裝好的 Raspberry Pi Imager，
* 這時請把你的 SD 卡插入讀卡機並連接到電腦，確定電腦有抓到這個碟，
* 接著選擇 OS 與安裝的磁碟，請務必挑選讀卡機的 SD Card，
* 都確定後就按 [WRITE](#link){: .btn .btn--primary } ，接下來就看你 SD Card 的速度等級嘍，筆者我的卡太老舊，在這可是等了不少時間。

<figure>
  <img src="{{ '/assets/images/raspberrypi/raspberry-pi-imager.png' | relative_url }}" alt="Raspberry Pi Imager">
</figure>
洗完澡回來就看到成功的畫面如下
<figure>
  <img src="{{ '/assets/images/raspberrypi/raspberry-pi-imager-successful.png' | relative_url }}" alt="Raspberry Pi Imager Successful">
</figure>

如同畫面說的，請把記憶卡拿出來吧！沒錯，系統就這樣安裝好了，可以把卡插到樹莓派的板子上了。

#### 3. 連線與設定

這時先把 Micro HDMI 連接到螢幕，鍵盤滑鼠也都接上，網路可以進入桌面後再設定，都確認接好後，接上電源吧。
成功進入系統了，接著就依照說明設定語系與網路吧。

<figure>
  <img src="{{ '/assets/images/raspberrypi/raspberry-pi-configuration.png' | relative_url }}" alt="Raspberry Pi Configuration">
</figure>

### 安裝Raspbian(方法二)
這個方法可以參考官網的 [Using other tools](https://www.raspberrypi.org/documentation/installation/installing-images/README.md) ，這裡筆者準備了第二張卡，是新買的 32GB ，當時賣家告知之前有客戶買了某某大廠的卡，但樹莓派卻挑卡讀不到！於是選了另一家大廠的卡，想說我的 8GB 舊卡都可以安裝了，不會這麼雷吧！接下來就按照下列程序安裝嘍。

#### 1. 下載必要軟體
* [7-Zip](http://www.7-zip.org/) (Windows)。
* [Raspbian Buster with desktop and recommended software](https://downloads.raspberrypi.org/raspbian_full_latest)，如下圖下載 Raspbian 壓縮檔。
* [balenaEtcher](https://www.balena.io/etcher/) , [Win32DiskImager](https://sourceforge.net/projects/win32diskimager/) 或 [imgFlasher](https://www.upswift.io/imgflasher/) 擇一下載，後面我們用 balenaEtcher 來介紹。

<figure>
  <img src="{{ '/assets/images/raspberrypi/raspberry-pi-raspbian-download-zip.png' | relative_url }}" alt="">
  <figcaption>下載Raspbian壓縮檔</figcaption>
</figure>

#### 2. 安裝 7-Zip
因為 Raspbian 映像檔超過 4GB (解壓後來到了 6.82GB )，且使用 ZIP64 格式，所以解壓的工具必需有支援 ZIP64 ，而 7-Zip 也是我慣用的壓縮工具，打開剛下載的 7-Zip 執行安裝，這裡就省略怎麼安裝 7-Zip 嘍。

#### 3. 解壓縮 Raspbian
剛下載的映像檔名像 `2020-02-13-raspbian-buster-full.zip`，用 7-Zip 解壓縮到你指定的路徑，在這個路徑下會有個檔像 `2020-02-13-raspbian-buster-full.img` 這樣。

#### 4. 安裝 balenaEtcher
打開剛下載的燒錄程式 `balenaEtcher-Setup-1.5.79.exe` 進行安裝，安裝好後啟動該程式如下圖。 
<figure>
  <img src="{{ '/assets/images/raspberrypi/raspberry-pi-etcher.png' | relative_url }}" alt="">
</figure>

#### 5. 開啟 balenaEtcher
* 選取映像檔
* 選取 SD Card，記得把卡插到讀卡機並連接到電腦。
* 上述二個都確認後，按 [Flash!](#link){: .btn .btn--info} 開始燒錄。


<figure class="half">
  <a href="/assets/images/raspberrypi/raspberry-pi-etcher-format.png"><img src="/assets/images/raspberrypi/raspberry-pi-etcher-format.png"></a>
  <a href="/assets/images/raspberrypi/raspberry-pi-etcher-flashing.png"><img src="/assets/images/raspberrypi/raspberry-pi-etcher-flashing.png"></a>
  <figcaption>Step 1 - 左圖，選取好映像檔跟記憶卡；Step 2 - 右圖，燒錄中。</figcaption>
</figure>

<figure class="half">
  <a href="/assets/images/raspberrypi/raspberry-pi-etcher-validating.png"><img src="/assets/images/raspberrypi/raspberry-pi-etcher-validating.png"></a>
  <a href="/assets/images/raspberrypi/raspberry-pi-etcher-complete.png"><img src="/assets/images/raspberrypi/raspberry-pi-etcher-complete.png"></a>
  <figcaption>Step 3 - 左圖，檢驗中；Step 4 - 右圖，完成。</figcaption>
</figure>

看到 Complete 就大功完成了，接下來就把記憶卡抽出來並插到樹莓派嘍。

**提醒：**還是要正確的從工作列右下角的圖示安全地移除硬體並退出媒體，避免你的記憶卡有損傷。
{: .notice--info}

## 心得
一開始準備了一張 8GB 的 SD Card，相當有年份的一張卡，它的燒錄速度真的很慢，約20多分鐘，但系統都安裝好後，系統效能的反應還能接受。

在安裝時，一開始使用方法二，但到 Validating 的作業時卻跳出錯誤，後來改用另一個燒錄程式 (Win32DiskImager) 就可以，不過上述的方法筆者試過也都正常。方法二的解壓映像檔方式，要注意的事是 SD Card 要先格式化 (Format) ，及解壓的軟件要慎選，官方寫說必須使用支援 ZIP64 ，後來使用 7zip 解壓縮就正常。

方法一與方法二的安裝內容是有差異的，方法一的 Programming 程式多了一些，若要輕量客制則選方法二，若要研究且省點事則方法一嘍！

## 參考文章 ##
