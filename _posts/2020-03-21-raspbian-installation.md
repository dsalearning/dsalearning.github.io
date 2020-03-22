---
title:  "樹莓派安裝Raspbian"
excerpt: "Raspberry Pi 4 出來了，和之前的版本最大的改變是多了 2GB/4GB 的記憶體版本可供選擇！"
header:
  teaser: assets/images/raspberry-pi-4-model-b.jpg
sidebar:
  - title: "進修課程名稱"
    text: "物聯網樹莓派通訊程式與影像運用實務班"
  - title: "進修日期"
    text: "2020/03/14~2020/05/23"
  - title: "講師"
    text: "徐國堂"
search: false
categories: 
  - AIoT
tags:
  - Raspberry Pi
  - 樹莓派
  - Raspbian
  - 物聯網
  - 安裝
last_modified_at: 2020-03-21T21:00-00:00
toc: true
---

<figure class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/raspberry-pi-4-model-b.jpg" alt="">
  <figcaption><a href="https://www.raspberrypi.org/blog/raspberry-pi-4-on-sale-now-from-35/" title="Raspberry Pi 4 on sale now from $35">圖片來源：Raspberry Pi 4 on sale now from $35</a>.</figcaption>
</figure> 

## 事前準備
* [Raspberry Pi 4 Model B](https://www.raspberrypi.com.tw/28040/raspberry-pi-4-model-b/) ： 和之前的版本最大的改變是多了 2GB/4GB 的記憶體版本可供選擇，本次使用的是4GB版。
* 記憶卡(SD Card) : 建議容量16GB以上[^sd-cards]，速度等級建議Class 10。系統安裝最小建議是8GB，但安裝完系統後僅剩1GB，想再安裝其它軟體就很困難了。
* 讀卡機 : 準備利用電腦把樹莓派影像檔燒錄到SD Card。
* 電源接頭(Power) : 使用 USB-C，使用 5V/3A 以上電源供應器才能穩定使用。
* 視訊接頭(Video) :  type-D (micro) HDMI 接頭，micro HDMI to HDMI cable。
* 螢幕 : 利用上速的視訊接頭，把樹莓派的系統影像輸出到螢幕，這樣比較方便設定。僅安裝後的第一次需要，之後就利用遠端桌面的工具登入操作。

[^sd-cards]: [the SD card requirements](https://www.raspberrypi.org/documentation/installation/sd-cards.md).


## Windows篇
* Windows 10 專業版 x64

### 安裝Raspbian(方法一)

一切都準備好了，開始來安裝[^installation]吧。

[^installation]: [installation guide](https://www.raspberrypi.org/documentation/installation/installing-images/README.md).

#### 1. 下載樹莓派影像檔
至[官網](https://www.raspberrypi.org/downloads/)下載最新的樹莓派影像檔（Raspberry Pi Imager)並安裝它。

#### 2. 執行樹莓派影像檔
* 打開剛安裝好的Raspberry Pi Imager，
* 這時請把你的SD卡插入讀卡機並連接到電腦，確定電腦有抓到這個碟，
* 接著選擇OS與安裝的磁碟，請務必挑選讀卡機的SD Card，
* 都確定後就按[WRITE](#link){: .btn .btn--primary }，接下來就看你SD Card的速度等級嘍，筆者我的卡太老舊，在這可是等了不少時間。

<figure>
  <img src="{{ '/assets/images/raspberry-pi-imager.png' | relative_url }}" alt="Raspberry Pi Imager">
</figure>
洗完澡回來就看到成功的畫面如下
<figure>
  <img src="{{ '/assets/images/raspberry-pi-imager-successful.png' | relative_url }}" alt="Raspberry Pi Imager Successful">
</figure>

如同畫面說的，請把記憶卡拿出來吧！沒錯，系統就這樣安裝好了，可以把卡插到樹莓派的板子上了。

#### 3. 連線與設定

這時先把Micro HDMI連接到螢幕，鍵盤滑鼠也都接上，網路可以進入桌面後再設定，都確認接好後，接上電源吧。
成功進入系統了，接著就依照說明設定語系與網路吧。

<figure>
  <img src="{{ '/assets/images/raspberry-pi-configuration.png' | relative_url }}" alt="Raspberry Pi Configuration">
</figure>

### 安裝Raspbian(方法二)
待寫

## 安裝遠端桌面

為了能讓本機可以透過遠端連線至樹莓派，這裡選用的是 「Microsoft遠端桌面」，這樣你可以透過電腦、平板與手機等連線到樹莓派。 

在樹莓派的工作列打開終端機(Terminal)，如下圖

<img src="{{ '/assets/images/raspberry-pi-taskbar.png' | relative_url }}" alt="Raspberry Pi Taskbar">

輸入下列命令進行安裝。

```bash
$ sudo apt-get -y install xrdp
```

安裝完畢後，你也必須在電腦、平板與手機等安裝「Microsoft遠端桌面」（以下簡稱Client端），這時你需要知道樹莓派的IP才能遠端連線，請在樹莓派的終端機輸入下列指令。

```bash
$ ifconfig
```

看你是使用有線連接還是無線連接，有線就看 `eth0` 的IP，無線就看 `wlan0` 的IP。

<figure>
  <img src="{{ '/assets/images/raspberry-pi-get-ipconfig.png' | relative_url }}" alt="Raspberry Pi ifconfig">
</figure>

當Client端的遠端桌面程式準備好與知道樹莓派的IP後，打開Client端的遠端桌面程式新增PC，樹莓派預設的登入帳號是 `pi` ，密碼是 `rsapberry` 。

這時你的樹莓派還連接著螢幕，而你的Client端也連入了樹莓派，就像遠端到Server一樣可以有各自的環境執行，不過Windows預設是有二個遠端連線，那樹莓派會有幾個呢？

## 心得
一開始準備了一張8GB的SD Card，相當有年份的一張卡，它的燒錄速度真的很慢，約20多分鐘，但系統都安裝好後，系統效能的反應還能接受。

在安裝時，一開始使用方法一，但到Verify的作業時卻跳出錯誤，後來改用方法二的解壓影像檔方式，要注意的事是SD Card要先格式化(Format)，及解壓的軟件要慎選，官方寫說必須使用支援ZIP64，後來使用7zip解壓縮就正常。

## 參考文章 ##
