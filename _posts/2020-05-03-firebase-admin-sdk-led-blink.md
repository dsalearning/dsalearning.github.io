---
title:  "Firebase雲端資料庫 - Firebase 遠端控制 LED燈亮"
permalink: /firebase/admin-sdk-led-blink/
excerpt: "樹莓派安裝 Firebase Admin SDK 後，就可以呼叫其 API 與設備溝通，達到遠端控制的目的，只要變更 Firebase 資料庫的屬性值就可以讓LED 燈亮與燈熄。"
header:
  teaser: assets/images/google/firebase-admin-sdk-led-blink.png
search: false
categories: 
  - 樹莓派
tags:
  - Firebase
  - Google
  - 雲端資料庫
  - Database
  - Python
  - LED
  - 待寫
sidebar:
  nav: "raspberry"
author_profile: false
last_modified_at: 2020-05-03T21:00-00:00
toc: true
---
<figure class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/google/firebase-admin-sdk-led-blink.png" alt="">
</figure> 

[前一篇](/databases/firebase-admin-sdk-installation/)已安裝好 Admin SDK 了，且也已經建立了 Firebase 專案與 Realtime Database ，接下來本篇就結合「[樹莓派控制入門篇 - LED閃爍](/aiot/raspberry-rpi-gpio-led-blink/)」，變成遠端控制 LED 燈亮與燈熄，只要變更 Firebase 資料庫的屬性值就可以了。

## 前置準備
* [在樹莓派建立Python虛擬環境](/aiot/raspberry-pip3-create-env/)
* [樹莓派控制入門篇 - LED閃爍](/aiot/raspberry-rpi-gpio-led-blink/)
* [Firebase雲端資料庫 - 建立 Firebase 專案](/databases/firebase-create-project/)
* [Firebase雲端資料庫 - Realtime Database安裝](/databases/firebase-realtime-db-installation/)
* [Firebase雲端資料庫 - 樹莓派安裝Admin SDK](/databases/firebase-admin-sdk-installation/)

## 教具準備

| 名稱 | 數量 | 規格 | 備註 | 
|:-------|:-----:|:----|:-----| 
| 樹莓派 | 1 | Raspberry Pi 4 Model B (4G) | 就只是現在頂規是這個版本，用三代的也可以 |
| 麵包板 | 1 | 不限 | 測試時容易連結 |
| 杜邦線 | 1 | 不限 | 保證沒有斷芯就好 |
| LED燈 | 1 | 不限 | 測試此設備能發光閃爍 |
| 電阻 | 1 | 10K | 為了不讓 LED 燒掉 |

## 作業環境
* 樹莓派：Raspbian
* Windows 10 專業版 x64

## 電路圖
待補

如下圖的[影片]()

## 身份驗證

經過前面幾篇文章的操作，我們已經

[^admin_privileges]: Authenticate with admin privileges(https://firebase.google.com/docs/database/admin/start?authuser=2#authenticate-with-admin-privileges)

<figure class="align-center">
  <a href="/assets/images/google/firebase-admin-sdk-key-json-pi.png"><img src="{{ site.url }}{{ site.baseurl }}/assets/images/google/firebase-admin-sdk-key-json-pi.png" alt=""></a>
</figure> 

## 範例

```python
#!user/bin/python3.7

import RPi.GPIO as GPIO
import firebase_admin
from firebase_admin import credentials
from firebase_admin import db

def setup():
    GPIO.setmode(GPIO.BCM)
    GPIO.setup(25,GPIO.OUT)
    
    # Fetch the service account key JSON file contents
    cred = credentials.Certificate('/home/pi/Documents/certificate/aiotfarm-firebase-adminsdk-5pv7h-0f26688b17.json')
    
    # Initialize the app with a service account, granting admin privileges
    firebase_admin.initialize_app(cred, {
        'databaseURL': 'https://aiotfarm.firebaseio.com/'
    })
    
    # As an admin, the app has access to read and write all data, regradless of Security Rules
    ledRef = db.reference('Demo/status')
    ledRef.listen(listener)         

def listener(event):
    print(event.data)
    if event.data:
        GPIO.output(25,GPIO.HIGH)
    else:
        GPIO.output(25,GPIO.LOW)

if __name__=="__main__":
    setup()
```

{% include video id="-suTMOxXOyA" provider="youtube" %}

## 參考文章