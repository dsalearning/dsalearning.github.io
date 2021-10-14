---
title:  "樹莓派安裝Jupyter Notebook"
permalink: /raspberry/install-jupyter-1/
excerpt: "Jupyter Notebook對於熟悉Python與數據分析的人員來說是必備的工具，一般都是在安裝電腦上，而樹莓派也支援這個工具的安裝！"
header:
  teaser: assets/images/raspberrypi/raspberry-pi-install-jupyter-notebook.png
search: false
categories: 
  - 樹莓派
tags:
  - IoT
  - 物聯網
  - JupyterNotebook
  - 安裝
sidebar:
  nav: "raspberry"
author_profile: false
last_modified_at: 2020-03-23T21:00-00:00
toc: true
---
<figure class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/raspberrypi/raspberry-pi-install-jupyter-notebook.png" alt="">
</figure> 

## 前置準備
* [樹莓派安裝Raspbian作業系統（Windows篇）](/aiot/raspberry-raspbian-1-installation/)

## 檢查 Python 版本
在安裝好樹莓派時，內建就有 Python 了，只是預設的版本是多少？路徑又在哪？我們來看看
```bash
$ python --version
$ which python
```
結果如下
```
pi@raspberrypi:~ $ python --version
Python 2.7.16
pi@raspberrypi:~ $ which python
/usr/bin/python
```
我們看到版本是 `2.7.16` 版，路徑在 `/usr/bin/python` 下，那有沒有 Python3 呢？ 版本又是多少？ 接著看嘍
```bash
$ python3 --version
$ which python3
```
結果如下
```
pi@raspberrypi:~ $ python3 --version
Python 3.7.3
pi@raspberrypi:~ $ which python3
/usr/bin/python3
```
所以都已經內建了，只是預設的版本是 `python2.7`，所以 `pip` 指令也就是 `python2.7` 的環境。

## 前置安裝
### 1. 登入root系統管理員帳號
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
這時 `pip` 的環境仍是 `python2.7`
```bash
$ pip --version
```
結果如下
```
pi@raspberrypi:~ $ pip3 install --upgrade pip
Looking in indexes: https://pypi.org/simple, https://www.piwheels.org/simple
Collecting pip
  Downloading https://files.pythonhosted.org/packages/54/0c/d01aa759fdc501a58f431eb594a17495f15b88da142ce14b5845662c13f3/pip-20.0.2-py2.py3-none-any.whl (1.4MB)
    100% |████████████████████████████████| 1.4MB 234kB/s 
Installing collected packages: pip
Successfully installed pip-20.0.2
pi@raspberrypi:~ $ pip --version
pip 18.1 from /usr/lib/python2.7/dist-packages/pip (python 2.7)
```

### 4. 重開機
```bash
$ reboot
```
重開機後，再檢查一下 `pip` 的環境是 `python2.7` 呢？ 還是 `python3.7` ？
```
pi@raspberrypi:~ $ python --version
Python 2.7.16
pi@raspberrypi:~ $ pip --version
pip 20.0.2 from /home/pi/.local/lib/python3.7/site-packages/pip (python 3.7)
```
嗯，所以接下來輸入 `pip` 就等於是 `pip3` 了。因為接下來我們都是使用 `python3.7` ，所以這樣方便了點。

## 安裝Jupyter Notebook

上述若是採用遠端連線則請重新連線進入，因為目前是使用 `pi` 帳號登入，所以開啟終端機輸入下列命令，記得要打 `sudo` 來摸擬系統管理員。
```bash
$ sudo pip3 install jupyter
```
不過筆者也有試過不用 `sudo` 安裝，安裝的指令是 `$ pip install jupyter` ，記住這時的 `pip` 是 `pip3` ，安裝時會有一些 WARNING 出現，但也是可以正常開啟 `jupyter notebook` ，執行程式也是正常，但曾有遇過 `kernel` 連接失敗的情況，不是很清楚是什麼情況，所以建議還是用 `sudo` 來安裝。要注意的是請用上述的命令安裝，若是用 `$ sudo pip install jupyter` 就會收到下列的錯誤。
```
jupyter-console requires Python '>=3.5' but the running Python is 2.7.16
```

## 開啟Jupyter Notebook
```bash
$ jupyter notebook
```
這時會在樹莓派自動開啟瀏覽器進入 Jupyter Notebook，也可以複製URL到本機電腦，但必須先修改設定檔才能遠端連結，修改的說明如下一個步驟。
<figure class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/raspberrypi/raspberry-pi-jupyter-notebook.png" alt="">
</figure> 

## 心得
在安裝Jupyter Notebook曾遇到過，可以正常啟動，但是執行時並沒有把執行結果輸出到畫面。因為在安裝時有出現下列的錯誤
```
ImportError: cannot import name 'create_prompt_application' from 'prompt_toolkit.shortcuts' (/usr/local/lib/python3.7/dist-packages/prompt_toolkit/shortcuts/__init__.py)
```
解決的方式就是執行下列的命令
```bash
$ sudo pip3 install 'prompt-toolkit==1.0.15'
```
這時再重新啟動Jupyter Notebook，就可能正常的輸出了，原因可能是沒有執行本篇的安裝前準備吧。

## 參考文章 ##
1. [Jupyter Notebook on Raspberry Pi](https://www.instructables.com/id/Jupyter-Notebook-on-Raspberry-Pi/)
2. [安裝jupyter notebook 於Raspberry Pi](http://blog.ittraining.com.tw/2018/10/jupyter-notebook-raspberry-pi-3.html)