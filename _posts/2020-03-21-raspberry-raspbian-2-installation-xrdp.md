---
title:  "樹莓派安裝 Microsort 遠端桌面"
permalink: /raspberry/2-installation-xrdp/
excerpt: "管理電腦伺服器時，不是走到那台電腦的位置，而是遠端連線到那台電腦操控，而 Microsoft 遠端桌面就是筆者日常管理常使用的工具之一。"
header:
  teaser: assets/images/raspberrypi/raspberry-pi-rdp-add-pc.png
search: false
categories: 
  - 樹莓派
tags:
  - IoT
  - 遠端桌面
  - 物聯網
  - 安裝
sidebar:
  nav: "raspberry"
author_profile: false
last_modified_at: 2020-03-24T21:00-00:00
toc: true
---
## 前置準備
* [樹莓派安裝Raspbian作業系統（Windows篇）](/aiot/raspberry-raspbian-1-installation/)

## 安裝遠端桌面

為了能讓本機可以透過遠端連線至樹莓派，這裡選用的是「 Microsoft 遠端桌面」，這樣你可以透過電腦、平板與手機等（以下簡稱 Client 端）連線到樹莓派。 

在樹莓派的工作列打開終端機(Terminal)，如下圖
<img src="{{ '/assets/images/raspberrypi/raspberry-pi-taskbar.png' | relative_url }}" alt="Raspberry Pi Taskbar">

輸入下列命令進行安裝。

```bash
$ sudo apt-get -y install xrdp
```

安裝完畢後，你也必須在 Client 端安裝「 Microsoft 遠端桌面」，這時你需要知道樹莓派的IP才能遠端連線，請在樹莓派的終端機輸入下列指令。

```bash
$ ifconfig
```

看你是使用有線連接還是無線連接，有線就看 `eth0` 的IP，無線就看 `wlan0` 的IP。

<figure>
  <img src="{{ '/assets/images/raspberrypi/raspberry-pi-get-ipconfig.png' | relative_url }}" alt="Raspberry Pi ifconfig">
</figure>

當 Client 端的遠端桌面程式準備好與知道樹莓派的IP後，打開 Client 端的遠端桌面程式新增 PC ，如下圖
<figure>
  <img src="{{ '/assets/images/raspberrypi/raspberry-pi-rdp-add-pc.png' | relative_url }}" alt="">
</figure>
樹莓派預設的登入帳號是 `pi` ，密碼是 `rsapberry` ，這裡的Session我選擇的是 `Xorg` 。
<figure class="half">
  <a href="/assets/images/raspberrypi/raspberry-pi-rdp-login.png"><img src="/assets/images/raspberrypi/raspberry-pi-rdp-login.png"></a>
</figure>

這時你的樹莓派還連接著螢幕，而你的 Client 端也連入了樹莓派，就像遠端到 Server 一樣可以有各自的環境執行，不過 Windows 預設是有二個遠端連線，那樹莓派會有幾個呢？

## 心得
就跟在本機建立VM一樣，設備不用再連接螢幕就可以管理與操作該電腦，是管理電腦的必備工具，這個一定要安裝起來。


## 參考文章 ##
