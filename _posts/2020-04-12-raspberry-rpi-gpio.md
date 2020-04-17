---
title:  "樹莓派 RPi.GPIO 使用說明"
excerpt: "GPIO（General Purpose I/O Ports）意思是通用輸入/輸出埠，也就是一些針腳可以通過它們輸出高低電平或者通過它們讀入針腳的狀態，以達控制硬體設備的功能。"
header:
  teaser: assets/images/raspberrypi/raspberry-pi-GPIO-Header-with-Photo-702x336.png
search: false
categories: 
  - AIoT
tags:
  - RaspberryPi
  - 樹莓派
  - Python
  - 物聯網
  - Thonny
  - GPIO
  - LED
last_modified_at: 2020-04-02T21:00-00:00
toc: true
---

學習樹莓派的第一個模組就是 `RPi.GPIO` ，什麼是 GPIO ? GPIO（General Purpose I/O Ports）意思是通用輸入/輸出埠，也就是一些針腳可以通過它們輸出高低電平或者通過它們讀入針腳的狀態，以達控制硬體設備的功能，例如控制 LED 燈開燈熄、蜂嗚器的聲響等，所以掌握了 GPIO 就相當於掌握了硬體的控制。

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
若去 print() 該屬性則會得到值為 `0` ，也就是 boolean 值的 False，所以也可以用 `False` 或 `0` 填入，但為了讓程式的可讀性好，請還是使用屬性名稱的方式。

`GPIO.IN` 想要獲取針腳的狀態就將針腳設定為輸入，

`initial`



### input()

### output()

### HIGH與LOW

### cleanup()

### setwarnings()



[^rpi-gpio]: [A Python module to control the GPIO on a Raspberry Pi](https://sourceforge.net/p/raspberry-gpio-python/wiki/BasicUsage/)

## 心得


## 參考文章

