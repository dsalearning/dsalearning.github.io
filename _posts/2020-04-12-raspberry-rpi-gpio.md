---
title:  "樹莓派 RPi.GPIO 使用說明"
permalink: /raspberry/rpi-gpio/
excerpt: "GPIO（General Purpose I/O Ports）意思是通用輸入/輸出埠，也就是一些針腳可以通過它們輸出高電位或者低電位，掌握了 GPIO 就相當於掌握了硬體的控制。"
header:
  teaser: assets/images/raspberrypi/raspberry-pi-GPIO-Header-with-Photo-702x336.png
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
  - 待寫
sidebar:
  nav: "raspberry"
author_profile: false
last_modified_at: 2020-04-19T21:00-00:00
toc: true
---

學習樹莓派的第一個模組就是 `RPi.GPIO` ，什麼是 GPIO ? GPIO（General Purpose I/O Ports）意思是通用輸入/輸出埠，也就是一些針腳可以通過它們輸出高電位或者低電位，可以供使用者由程式控制自由使用，與裝置進行通訊，達到控制裝置的目的。既然一個針腳可以用於輸入、輸出或其他特殊功能，那麼一定有暫存器用來選擇這些功能。對於輸入，一定可以透過讀取某個暫存器來確定針腳電位的高低；對於輸出，一定可以透過寫入某個暫存器來讓這個針腳輸出高電位或者低電位；對於其他特殊功能，則有另外的暫存器來控制它們，所以掌握了 GPIO 就相當於掌握了硬體的控制。

<figure class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/raspberrypi/raspberry-pi-GPIO-Header-with-Photo-702x336.png" alt="">
  <figcaption><a href="https://www.raspberrypi-spy.co.uk/2012/06/simple-guide-to-the-rpi-gpio-header-and-pins/" title="Raspberry Pi 40-pin GPIO Layout">圖片來源：Simple Guide to the Raspberry Pi GPIO Header</a>.</figcaption>
</figure> 
上圖有 40 根針腳，這就是樹莓派控制外部傳感器的接口，稱之為 GPIO(General Purpose I/O Port)。這裡可以看到有二種編號規則，分別是紅框內的 `BOARD` 編號規則，與紅框外的 `BCM` 編號規則，接下來我們就來試試 `RPi.GPIO` 的用法吧。


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
| LED燈 | 2 | 不限 | 測試此設備能發光閃爍 |
| 電阻 | 2 | 10K | 為了不讓 LED 燒掉 |

## 檢查 wiringPi 版本與升級

由於樹莓派因版本不同，其各針腳的定義也會有些許的差異，那是否能在樹莓派裡就得知呢？ 可以的，但你可能跟筆者一樣遇到 Pi 4B 版需要升級的情況，首先檢查一下版本。
```bash
$ gpio -v
```
回傳結果如下，是 `2.50` 版。
```
pi@raspberrypi:~ $ gpio -v
gpio version: 2.50
Copyright (c) 2012-2018 Gordon Henderson
This is free software with ABSOLUTELY NO WARRANTY.
For details type: gpio -warranty

Raspberry Pi Details:
  Type: Unknown17, Revision: 02, Memory: 0MB, Maker: Sony 
  * Device tree is enabled.
  *--> Raspberry Pi 4 Model B Rev 1.2
  * This Raspberry Pi supports user-level GPIO access.
```
查看各針腳編號的命令如下：
```bash
$ gpio readall
```
回傳結果如下
```
pi@raspberrypi:~ $ gpio readall
Oops - unable to determine board type... model: 17
```
這個問題是 `wiringPi` 模組版本的問題，所以如果你也跟筆者一樣是 Pi 4B版且 `wiringPi` 版本是 `2.50` 版，那麼就跟著筆者按照[官網指引](http://wiringpi.com/wiringpi-updated-to-2-52-for-the-raspberry-pi-4b/)升級即可，命令如下:
```none
cd /tmp
wget https://project-downloads.drogon.net/wiringpi-latest.deb
sudo dpkg -i wiringpi-latest.deb
```

更新好後，再來檢查一次版本
```bash
$ gpio -v
```
這時候看到版本已更新到 `2.52` 了。
```
pi@raspberrypi:/tmp $ gpio -v
gpio version: 2.52
Copyright (c) 2012-2018 Gordon Henderson
This is free software with ABSOLUTELY NO WARRANTY.
For details type: gpio -warranty

Raspberry Pi Details:
  Type: Pi 4B, Revision: 02, Memory: 4096MB, Maker: Sony 
  * Device tree is enabled.
  *--> Raspberry Pi 4 Model B Rev 1.2
  * This Raspberry Pi supports user-level GPIO access.
```
看看 Pi 4B 版的各腳位對應吧。
```bash
$ gpio readall
```
這時回傳的結果已可以看到 Pi 4B 版的各腳位編號了
```
pi@raspberrypi:/tmp $ gpio readall
 +-----+-----+---------+------+---+---Pi 4B--+---+------+---------+-----+-----+
 | BCM | wPi |   Name  | Mode | V | Physical | V | Mode | Name    | wPi | BCM |
 +-----+-----+---------+------+---+----++----+---+------+---------+-----+-----+
 |     |     |    3.3v |      |   |  1 || 2  |   |      | 5v      |     |     |
 |   2 |   8 |   SDA.1 | ALT0 | 1 |  3 || 4  |   |      | 5v      |     |     |
 |   3 |   9 |   SCL.1 | ALT0 | 1 |  5 || 6  |   |      | 0v      |     |     |
 |   4 |   7 | GPIO. 7 |   IN | 1 |  7 || 8  | 1 | ALT5 | TxD     | 15  | 14  |
 |     |     |      0v |      |   |  9 || 10 | 1 | ALT5 | RxD     | 16  | 15  |
 |  17 |   0 | GPIO. 0 |   IN | 0 | 11 || 12 | 0 | IN   | GPIO. 1 | 1   | 18  |
 |  27 |   2 | GPIO. 2 |   IN | 0 | 13 || 14 |   |      | 0v      |     |     |
 |  22 |   3 | GPIO. 3 |   IN | 0 | 15 || 16 | 0 | IN   | GPIO. 4 | 4   | 23  |
 |     |     |    3.3v |      |   | 17 || 18 | 0 | IN   | GPIO. 5 | 5   | 24  |
 |  10 |  12 |    MOSI | ALT0 | 0 | 19 || 20 |   |      | 0v      |     |     |
 |   9 |  13 |    MISO | ALT0 | 0 | 21 || 22 | 0 | IN   | GPIO. 6 | 6   | 25  |
 |  11 |  14 |    SCLK | ALT0 | 0 | 23 || 24 | 1 | OUT  | CE0     | 10  | 8   |
 |     |     |      0v |      |   | 25 || 26 | 1 | OUT  | CE1     | 11  | 7   |
 |   0 |  30 |   SDA.0 |   IN | 1 | 27 || 28 | 1 | IN   | SCL.0   | 31  | 1   |
 |   5 |  21 | GPIO.21 |   IN | 1 | 29 || 30 |   |      | 0v      |     |     |
 |   6 |  22 | GPIO.22 |   IN | 1 | 31 || 32 | 0 | IN   | GPIO.26 | 26  | 12  |
 |  13 |  23 | GPIO.23 |   IN | 0 | 33 || 34 |   |      | 0v      |     |     |
 |  19 |  24 | GPIO.24 |   IN | 0 | 35 || 36 | 0 | IN   | GPIO.27 | 27  | 16  |
 |  26 |  25 | GPIO.25 |   IN | 0 | 37 || 38 | 0 | IN   | GPIO.28 | 28  | 20  |
 |     |     |      0v |      |   | 39 || 40 | 0 | IN   | GPIO.29 | 29  | 21  |
 +-----+-----+---------+------+---+----++----+---+------+---------+-----+-----+
 | BCM | wPi |   Name  | Mode | V | Physical | V | Mode | Name    | wPi | BCM |
 +-----+-----+---------+------+---+---Pi 4B--+---+------+---------+-----+-----+
```

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

## RPi.GPIO方法與屬性
### 匯入模組
一起來了解 `RPi.GPIO`[^rpi-gpio] 這個模組的各個方法。匯入模組的指令如下：
```python
import RPi.GPIO as GPIO
```

### BOARD與BCM編號規則屬性
如上文針腳圖有紅框內的 `BOARD` 編號；與紅框外的 `BCM` 編號規則，此編號規則會因樹莓派版本而有些不同，所以使用時請務必留意。

打印出 `BCM` 編號規則屬性值
```python
print(GPIO.BCM)
```
`BCM` 回傳結果如下
```
11
```
打印出 `BOARD` 編號規則屬性值
```python
print(GPIO.BOARD)
```
BOARD 回傳結果如下
```
10
```

### setmode()
上述知道編號規則屬性值各是多少了，但記數值不直覺，所以還是用屬性名稱吧！

設定 BCM 編號規則的程式如下
```python
GPIO.setmode(GPIO.BCM)
```
設定 BOARD 編號規則的程式如下
```python
GPIO.setmode(GPIO.BOARD)
```


### getmode()
這個是取得被設置的編號規則屬性值
```python
mode = GPIO.getmode()
```
回傳的值如下 ：
* 未設定： None
* BCM： 11
* BOARD： 10

**範例**

查看未設定編號規則時是取得 `None` ，再設定指定的編號規則後，取得設定的屬性值。
```python
import RPi.GPIO as GPIO

print(GPIO.getmode())
print(GPIO.BCM)
print(GPIO.BOARD)
GPIO.setmode(GPIO.BCM)
mode = GPIO.getmode()
print(mode)
```
回傳結果
```
None
11
10
11
```

### setup()
在你要使用的針腳時，必須先設定這個針腳是作為**輸出**還是**輸入**。

**語法**
```python
GPIO.setup({channel | chan_list}, {GPIO.OUT | GPIO.IN}, [initial= {GPIO.HIGH | GPIO.LOW}])
```

**引數**

`channel` 是指針腳的編號，依據設定的針腳編號規則的編號，例如
```python
channel = 25 #BCM編號規則
```

`chan_list` 也可以一次設定多個針腳，例如
```python
chan_list = [11,12]
```

`GPIO.OUT` 將指定的針腳作為輸出，如果想讓 LED 燈亮或驅動某個設備，就需要給它們電流和電壓，只要將針腳設定為輸出即可，範例如下
```python
GPIO.setup(25, GPIO.OUT)
```
若去 print() 該屬性則會得到值為 `0` ，相當於是 boolean 值的 False，所以也可以用 `False` 填入，但為了讓程式的可讀性好，請還是使用屬性名稱的方式。
```python
print(GPIO.OUT)
print(type(GPIO.OUT))
```
回傳結果如下
```
0
<class 'int'>
```

`GPIO.IN` 想要獲取針腳的狀態就將針腳設定為輸入，筆者認為這些傳感器輸出訊號，在樹莓派要接收就設為輸入。設定都跟上面的 `GPIO.OUT` 語法一樣。

`initial` 為針腳設定預設值，例如下方範例為設定輸出的針腳同時給予高電位。
```python
GPIO.setup(channel, GPIO.OUT, initial=GPIO.HIGH)
```
相當於下面語法
```python
GPIO.setup(channel, GPIO.OUT)
GPIO.output(channel,GPIO.HIGH)
```

### output()
前面的範例已使用到 `output()` ，意思就是設定針對輸出的狀態是高電位還是低電位，這樣就可以讓 LED 燈亮或熄滅，或者驅動或停止驅動裝置。
```python
GPIO.output(channel,GPIO.HIGH)
```

### input()
當針腳已被設定高低電位時，可以用此函式取得該針腳的當下狀態為何，這樣就可以進一步有邏輯去判斷了。
```python
if GPIO.input(channel):
    print('HIGH')
else:
    print('LOW')
```

### cleanup()
好的程序猿就要懂得釋放資源，這樣才不會成為技術債或損壞樹莓派，釋放的命令如下：
```python
GPIO.cleanup()
```
**注意！** GPIO.cleanup()只會清理執行腳本裡使用過的 GPIO 腳位，也會清除正在使用的編號規則。
{: .notice--danger}

### setwarnings()
當檢測到某個針腳己經被設置為非預設值時，你會得到一個警告。如果要停用這個警告，就使用這個函式即可停止。
```python
GPIO.setwarnings(False)
```
例如筆者的第一個範例 [LED閃爍](/aiot/raspberry-rpi-gpio-led-blink/)，第一次執行時並沒有警告出現，當再按一次執行時，就會出現如下的警告：
```
blink.py:7: RuntimeWarning: This channel is already in use, continuing anyway.  Use GPIO.setwarnings(False) to disable warnings.
  GPIO.setup(25,GPIO.OUT,initial=GPIO.LOW)
```

### wait_for_edge()
待寫

### event_detected()
待寫

## 範例
### A. 使用 `chan_list` 
設定針腳 24,25 (BCM編號) 為輸出，同時設定這二個針腳為輸出，本範例是將 2 個 LED 一開始都亮 3 秒後輪流亮 1 秒。
```python
#實作 ： 2個LED都亮3秒後輪流亮1秒
import RPi.GPIO as GPIO
import time

chan_list = [24,25]
GPIO.setmode(GPIO.BCM)
GPIO.setup(chan_list,GPIO.OUT)
GPIO.output(chan_list, GPIO.HIGH) #全部燈亮
time.sleep(3)
while True:
    GPIO.output(chan_list, (GPIO.HIGH, GPIO.LOW)) #24 亮; 25 暗
    time.sleep(1))
    GPIO.output(chan_list, (GPIO.LOW, GPIO.HIGH)) #24 暗; 25 亮
    time.sleep(1)
```
{% include video id="WvCg-ieOsiY" provider="youtube" %}

[^rpi-gpio]: [A Python module to control the GPIO on a Raspberry Pi](https://sourceforge.net/p/raspberry-gpio-python/wiki/BasicUsage/)



## 參考文章

