---
title:  "本機連樹莓派Jupyter Notebook"
permalink: /raspberry/install-jupyter-2/
excerpt: "在樹莓派安裝了Jupyter Notebook後，若只能在樹莓派用就不方便了，本篇介紹如何修改設定可以讓本機也能連到樹莓派的Jupyter Notebook喔！"
header:
  teaser: assets/images/raspberrypi/raspberry-pi-jupyter-notebookapp-ip.png
search: false
categories: 
  - 樹莓派
tags:
  - IoT
  - 物聯網
  - JupyterNotebook
sidebar:
  nav: "raspberry"
author_profile: false
last_modified_at: 2020-03-22T21:00-00:00
toc: true
---
## 前置準備
* [樹莓派安裝Raspbian作業系統（Windows篇）](/aiot/raspberry-raspbian-1-installation/)
* [樹莓派安裝Jupyter Notebook](/aiot/raspberry-install-jupyter-1/)

### 1. 產生設定檔
```bash
$ jupyter notebook --generate-config
```
結果如下
```
pi@raspberrypi:~ $ jupyter notebook --generate-config
Writing default config to: /home/pi/.jupyter/jupyter_notebook_config.py
```
**注意：**執行上述的命令後，會建立出如上路徑的 `.py` 檔，等等就是要修改這個路徑的 `.py` 檔
{: .notice--info}

### 2. 確認是否產生檔案了
你可以輸入下列命令檢查該 `.py` 檔的權限
```bash
$ ls -al /home/pi/.jupyter/
```
應該看到下列的結果
```
pi@raspberrypi:~ $ ls -al /home/pi/.jupyter/
total 48
drwx------  2 pi pi  4096 Mar 22 21:10 .
drwxr-xr-x 20 pi pi  4096 Mar 22 23:15 ..
-rw-r--r--  1 pi pi 34411 Mar 22 20:54 jupyter_notebook_config.py
pi@raspberrypi:~ $ 
```

### 3. 修改設定檔
修改 `.py` 檔的c.NotebookApp.ip屬性值，在終端機輸入下列命令
```
$ sudo nano /home/pi/.jupyter/jupyter_notebook_config.py
```
* 按 [Ctrl](#link){: .btn .btn--primary } + [W](#link){: .btn .btn--primary }進到 Search列輸入 `localhost`，按 [Enter](#link){: .btn .btn--primary }。
* 將 `c.NotebookApp.ip='localhost'` 的 `localhost` 修改成 `0.0.0.0`，如下圖。

<figure>
  <img src="{{ '/assets/images/raspberrypi/raspberry-pi-jupyter-notebookapp-ip.png' | relative_url }}" alt="Raspberry Pi NotebookApp.ip">
</figure>

* 按 [Ctrl](#link){: .btn .btn--primary } + [O](#link){: .btn .btn--primary } 存檔。
* 按 [Ctrl](#link){: .btn .btn--primary } + [X](#link){: .btn .btn--primary } 離開。

這時重新啟動Jupyter Notebook，就可以將產生的URL貼在本機的瀏覽器執行嘍！

{% include video id="kQ3fgmYQ024" provider="youtube" %}


## 心得
因為樹莓派裡沒有安裝熟悉的輸入法，當本機也可以使用時，樹莓派裡的輸入法就不再那麼必要了，只要在本機的終端機SSH進樹莓派就可以啟動Jupyter Notebook，一切都在本機操作，就不需要連進樹莓派，這樣對我感覺就很方便。


## 參考文章 ##
1. [Jupyter Notebook on Raspberry Pi](https://www.instructables.com/id/Jupyter-Notebook-on-Raspberry-Pi/)
2. [安裝jupyter notebook 於Raspberry Pi](http://blog.ittraining.com.tw/2018/10/jupyter-notebook-raspberry-pi-3.html)