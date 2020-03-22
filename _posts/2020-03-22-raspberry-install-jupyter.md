---
title:  "樹莓派安裝Jupyter Notebook"
excerpt: "Jupyter Notebook對於熟悉Python與數據分析的人員來說是必備的工具，一般都是在安裝電腦上，而樹莓派也支援這個工具的安裝！"
header:
  teaser: assets/images/raspberry-pi-install-jupyter-notebook.png
search: false
categories: 
  - AIoT
tags:
  - Raspberry Pi
  - 樹莓派
  - 遠端桌面
  - 物聯網
  - Jupyter Notebook
  - 安裝
last_modified_at: 2020-03-22T21:00-00:00
toc: true
---
<figure class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/raspberry-pi-install-jupyter-notebook.png" alt="">
</figure> 

## 安裝前準備
### 1. 登入root帳號
```bash
$ sudo su -
```
會從 `pi` 帳號進入到 `root` 帳號
```
pi@raspberrypi:~ $ sudo su -
root@raspberrypi:~# 
```

### 2. 更新系統
這時已是使用root帳號了，所以下列的命令可以不用再打 `sudo` 嘍。
```bash
$ apt-get update
```

### 3. 更新pip
```bash
$ pip3 install --upgrade pip
```

### 4. 重開機
```bash
$ reboot
```

## 安裝Jupyter Notebook
上述若是採用遠端連線則請重新連線進入，因為目前是使用 `pi` 帳號登入，所以開啟終端機輸入下列命令，記得要打 `sudo` 來摸擬系統管理員。
```bash
$ sudo pip3 install jupyter
```

## 開啟Jupyter Notebook
```bash
$ jupyter notebook
```
這時會在樹莓派自動開啟瀏覽器進入 Jupyter Notebook，也可以複製URL到本機電腦，但必須先修改設定檔才能遠端連結，修改的說明如下一個步驟。
<figure class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/raspberry-pi-jupyter-notebook.png" alt="">
</figure> 


## 讓本機的電腦可以連樹莓派的Jupyter Notebook
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
  <img src="{{ '/assets/images/raspberry-pi-jupyter-notebookapp-ip.png' | relative_url }}" alt="Raspberry Pi NotebookApp.ip">
</figure>

* 按 [Ctrl](#link){: .btn .btn--primary } + [O](#link){: .btn .btn--primary } 存檔。
* 按 [Ctrl](#link){: .btn .btn--primary } + [X](#link){: .btn .btn--primary } 離開。

這時重新啟動Jupyter Notebook，就可以將產生的URL貼在本機的瀏覽器執行嘍！

{% include video id="kQ3fgmYQ024" provider="youtube" %}


## 心得
在安裝Jupyter Notebook後，可以正常啟動，但是執行時並沒有把執行結果輸出到畫面。

## 參考文章 ##
1. [Jupyter Notebook on Raspberry Pi](https://www.instructables.com/id/Jupyter-Notebook-on-Raspberry-Pi/)
2. [安裝jupyter notebook 於Raspberry Pi](http://blog.ittraining.com.tw/2018/10/jupyter-notebook-raspberry-pi-3.html)