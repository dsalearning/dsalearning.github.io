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
  - 物聯網
  - Jupyter Notebook
  - 安裝
last_modified_at: 2020-03-23T21:00-00:00
toc: true
---
<figure class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/raspberry-pi-install-jupyter-notebook.png" alt="">
</figure> 

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