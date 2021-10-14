---
title:  "初探樹莓派內建的 Thonny Python IDE"
permalink: /raspberry/thonny-python-ide/
excerpt: "Thonny 是專為初學者設計的 Python 集成開發環境，非常適合在樹莓派上學習 Python 時使用喔！"
header:
  teaser: assets/images/raspberrypi/raspberry-pi-thonny-python-ide.png
search: false
categories: 
  - 樹莓派
tags:
  - IoT
  - Python
  - 物聯網
  - Thonny
sidebar:
  nav: "raspberry"
author_profile: false
last_modified_at: 2020-04-02T21:00-00:00
toc: true
---
在安裝好樹莓派後，就內建了這個 Python IDE ，功能比起 Spyder 與 PyCharm 就相對功能簡易了些， 不過有程式編輯區、 Shell 視窗， 也有 Debug 模式，這點就非常實用了。接下來我們就來一探究竟吧！

## 前置準備
* [樹莓派安裝Raspbian作業系統（Windows篇）](/aiot/raspberry-raspbian-1-installation/)
* [在樹莓派建立Python虛擬環境](/aiot/raspberry-pip3-create-env/)

## 如何使用 Thonny
### 變更介面
在第一次開啟 Thonny 時，預設的 UI Mode 是 `simple` 模式，就看各人的選擇了，筆者是點選在視窗的右上角的 `Switch to rebular mode`。

<img src="{{ '/assets/images/raspberrypi/raspberry-pi-thonny-toolbar.png' | relative_url }}" alt="">

點選完並沒有看到變化，這時請**重新啟動 Thonny**，就會看到介面的不同了，若要切換不同的 UI Mode，則是在 `Tools` > `Options` > `General` 裡設定。
<figure class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/raspberrypi/raspberry-pi-thonny-ui-mode.png" alt="">
</figure> 
你也可以在這選擇介面的語言，所以筆者這裡就切換到中文嘍。

### 變更主題
以現在流行黑色主題的趨勢，且眼睛看了也是真的比較舒服啦， Thonny 也有黑色主題可以選喔~
<figure class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/raspberrypi/raspberry-pi-thonny-python-ide.png" alt="">
</figure> 
是吧！這個顏色的配置比 Spyder 的黑色主題看了更舒服，不過筆者為了介紹，所以會以預設的主題來介紹。

### 設定直譯器(Interpreter)
這裡的 Interpreter 翻譯成**直譯器**，且上圖的中文介面還譯成**直釋器**，這應該是錯字吧！哈哈，這不是筆者要介紹的，這裡的設定可是本篇最重要的呢，因為我們之前有介紹過怎麼在樹莓派建立 Python 的虛擬環境[^create-env]，那要怎麼在 Thonny 執行的 Script 是在指定的虛擬環境呢？ 那就要在這裡設定了，我們來看看怎麼設定吧。

[^create-env]: [在樹莓派建立Python虛擬環境](/aiot/raspberry-pip3-create-env/)


設定之前， Thonny 是預設在 Python 的預設環境，如下圖

<img src="{{ '/assets/images/raspberrypi/raspberry-pi-thonny-default-python.png' | relative_url }}" alt="">

我們之前已建立了 `dsalearning` 的這個虛擬環境[^create-env]，所以就來設定讓程式可以執行在 `dsalearning` 的環境。

* 開啟 Thonny options
請選 `Tool` > `Options` > `General` > `Interpreter` 
* 選取 `Alternative Python 3 interpreter or virtual environment`
在下拉選單選取如下圖 ，這時會出現 Detail 選項，點選 `Locate another python executable ...`
  <figure class="align-center">
    <img src="{{ site.url }}{{ site.baseurl }}/assets/images/raspberrypi/raspberry-pi-thonny-options-interpreter.png" alt="">
  </figure> 
選取虛擬環境所在的目錄 `bin` 底下的 `python3` 。 
  <figure class="align-center">
    <img src="{{ site.url }}{{ site.baseurl }}/assets/images/raspberrypi/raspberry-pi-thonny-options-interpreter-path.png" alt="">
  </figure> 
沒問題後就按 [OK](#link){: .btn .btn--primary }，這時在 Shell 視窗會看到指定的 Python 路徑。
  <figure class="align-center">
    <img src="{{ site.url }}{{ site.baseurl }}/assets/images/raspberrypi/raspberry-pi-thonny-shell.png" alt="">
  </figure> 

### 執行 Script
那接下來我們就來簡單寫個語法來執行看看嘍~
```python
if __name__ == '__main__':
    print('Hello, DSA')
```
結果如下
  <figure class="align-center">
    <img src="{{ site.url }}{{ site.baseurl }}/assets/images/raspberrypi/raspberry-pi-thonny-sample1.png" alt="">
  </figure> 
筆者也在終端機進入到虛擬環境去執行，也是正常的執行。
  <figure class="align-center">
    <img src="{{ site.url }}{{ site.baseurl }}/assets/images/raspberrypi/raspberry-pi-thonny-sample1-bash.png" alt="">
  </figure> 

## 心得
能夠選擇要運行的虛擬環境真的很方便，跟 Anaconda 一樣可以選擇虛擬環境，但又沒有 Anaconda 那麼佔資源，但在樹莓派裡開發 python 就有點怪了，如果可以在本機的環境連到樹莓派的虛擬環境就很方便了，課堂上老師用 PyCharm IDE 專業版是可以做到，但這個版本是要付費的，只好再來研究 Spyder 看看嘍。

## 參考文章

