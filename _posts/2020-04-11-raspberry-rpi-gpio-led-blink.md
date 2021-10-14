---
title:  "樹莓派控制入門篇 - LED閃爍"
permalink: /raspberry/rpi-gpio-led-blink/
excerpt: "控制樹莓派的 GPIO ，連結到麵包板上讓 LED 燈閃爍，這個 Lab 算是接觸硬體程式的 Hello World了，本次學習是控制 LED 亮一秒暗一秒的簡單控制。"
header:
  teaser: assets/images/raspberrypi/lab/led-blink.png
search: false
categories: 
  - 樹莓派
tags:
  - IoT
  - Python
  - 物聯網
  - Thonny
  - GPIO
  - LED
sidebar:
  nav: "raspberry"
author_profile: false
last_modified_at: 2020-04-02T21:00-00:00
toc: true
---

學程式語言第一個程式大概都是列印或顯示出 Hello World ，而學物聯網第一個實作大概都是學習 LED 控制，算是硬體編程的 Hello World，來試試吧！本學習是用 Python 語言開發，匯入 RPi.GPIO 模組來控制 LED 燈的亮燈與熄滅，做為學習樹莓派的入門篇，所以會多做說明。

## 前置準備
* [樹莓派安裝Raspbian作業系統（Windows篇）](/aiot/raspberry-raspbian-1-installation/)
* [在樹莓派建立Python虛擬環境](/aiot/raspberry-pip3-create-env/)
* [樹莓派安裝 Microsort 遠端桌面](/aiot/raspberry-raspbian-2-installation-xrdp/)

## 教具準備

| 名稱 | 數量 | 規格 | 備註 | 
|:-------|:-----:|:----|:-----| 
| 樹莓派 | 1 | Raspberry Pi 4 Model B (4G) | 就只是現在頂規是這個版本，用三代的也可以 |
| 麵包板 | 1 | 不限 | 測試時容易連結 |
| 杜邦線 | 1 | 不限 | 保證沒有斷芯就好 |
| LED燈 | 1 | 不限 | 測試此設備能發光閃爍 |
| 電阻 | 1 | 10K | 為了不讓 LED 燒掉 |

## 安裝 RPi.GPIO

不管用什麼方式，請在樹莓派的 **Python 虛擬環境**安裝 `RPi.GPIO` 模組，上述前置準備可以學習到怎麼連到此環境，安裝命令可以參考[官方 RPI.GPIO 連結](https://pypi.org/project/RPi.GPIO/)，命令如下 ：
```bash
(dsalearning) $ pip install RPi.GPIO
```
安裝結果如下
```
(dsalearning) pi@raspberrypi:~/Documents/envs/dsalearning $ pip install RPi.GPIO
Looking in indexes: https://pypi.org/simple, https://www.piwheels.org/simple
Collecting RPi.GPIO
  Using cached https://www.piwheels.org/simple/rpi-gpio/RPi.GPIO-0.7.0-cp37-cp37m-linux_armv7l.whl (69 kB)
Installing collected packages: RPi.GPIO
Successfully installed RPi.GPIO-0.7.0
```
看到 `Successfully installed` 就來檢查一下虛擬環境已匯入的模組是否有 `RPi.GPIO`
```bash
(dsalearning) $ pip list
```
檢查結果
```
(dsalearning) pi@raspberrypi:~/Documents/envs/dsalearning $ pip list
Package    Version
---------- -------
pip        20.0.2 
RPi.GPIO   0.7.0  
setuptools 46.1.3 
wheel      0.34.2 
```
嗯，確定是已安裝進來了，可以放心的使用嘍~

## 連線圖
在開始連接硬體電路之前，**請把樹莓派關機，並斷開電源**，因為樹莓派主板帶電時，進行插接電路很可能會導致電子元件的燒毀，筆者在學習時，旁邊座的是讀 Double E 的大哥，總是提醒我這件事，誰叫我一個 E 都沒有，只好找時間進修了。連接如下圖。
<figure class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/raspberrypi/lab/led-blink.png" alt="">
  <figcaption>LED 電路連線圖</figcaption>
</figure> 
接線要注意的是 LED 長針 (正極) 黃色線連接主板的 GPIO ，這裡筆者是接 GPIO25 ([BCM編號](/aiot/raspberry-rpi-gpio/#board與bcm編號規則屬性))，短針 (負極) 綠色線接電阻與接地 (Ground) 串聯，筆者有另外買 T-Cobbler Plus for Paspberry Pi(以下簡稱 T 型接頭)，接線就不用這樣數腳位了。

**注意！** 連接硬體電路之前，請把樹莓派關機，並斷開電源，因為很重要，所以再說一次。
{: .notice--danger}

## 程式開發
連線好後，我們就用 Python 來開發程式，而 Python 的 IDE 筆者接觸過的就有 Spyder 、 PyCharm 與 VS Code ，不過這裡筆者是使用樹莓派內建的 Thonny ，可以參考本站的「[初探樹莓派內建的 Thonny Python IDE](/aiot/raspberry-thonny-python-ide/)」，直接在樹莓派裡開發，這個 IDE 也是很不錯喔，所以請開啟 Thonny 吧，因為買來的 T 型接頭是 `BCM` 編號規則的，授課的老師也是教 `BCM` ，所以接下來的 Python 也就是用 `BCM` 的編號規則。。

範例程式碼如下
```python
# 實作 : 亮一秒暗一秒
import RPi.GPIO as GPIO
import time

def setup():
    GPIO.setmode(GPIO.BCM)
    GPIO.setup(25,GPIO.OUT)
    print("OK")

def blink():
    while True:
        GPIO.output(25,GPIO.HIGH)
        time.sleep(1)
        GPIO.output(25,GPIO.LOW)
        time.sleep(1)

if __name__=="__main__":
    setup()
    blink()
```

迫不急待的可以先貼碼試試，把程式上傳到樹莓派，在 Thonny 就只要按 [F5](#link){: .btn .btn--primary} 執行即可。
{% include video id="APghrzah6_U" provider="youtube" %}

如果想更進一步了解 `RPi.GPIO` 模組，請參考下一篇「[樹莓派 RPi.GPIO 使用說明](/aiot/raspberry-rpi-gpio/)」。

## 參考文章
1. [A Python module to control the GPIO on a Raspberry Pi](https://sourceforge.net/p/raspberry-gpio-python/wiki/BasicUsage/)
