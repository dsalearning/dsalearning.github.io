---
title: "Command 'python' not found！解決 python 是 python3"
permalink: /linux/linux-xrdp-install/
excerpt: "安裝了 python3，呼叫 python 時，卻告訴你 python not found！只要安裝了 python-is-python3 即可。"
header:
  teaser: /assets/images/linux/linux-python-is-python3.png
last_modified_at: 2021-11-23T23:00:00-00:00
toc: true
categories:
  - Linux
tags:
  - python
# sidebar:
#   nav: "linux"
author_profile: true
---

<figure class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/linux/linux-python-is-python3.png" alt="">
</figure> 

## 作業環境
* Ubuntu 20.04.3 LTS
* Windows 10 專業版

## 問題
在 `Ubuntu 20.04` 已經安裝了 python3 ，但呼叫 `python` 指令時，卻收到下列訊息
```bash
$ python

Command 'python' not found, did you mean:

  command 'python3' from deb python3
  command 'python' from deb python-is-python3
```
但輸入 `python3` 是可以進入
```bash
$ python3
Python 3.8.10 (default, Sep 28 2021, 16:10:42) 
[GCC 9.3.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> 
```

## 解決方式
安裝 `python-is-python3`，指令如下
```bash
$ sudo apt install python-is-python3
```
安裝結果如下
```bash
正在讀取套件清單... 完成
正在重建相依關係          
正在讀取狀態資料... 完成
以下套件為自動安裝，並且已經無用：
  libllvm11
使用 'sudo apt autoremove' 將之移除。
下列【新】套件將會被安裝：
  python-is-python3
升級 0 個，新安裝 1 個，移除 0 個，有 39 個未被升級。
需要下載 2,364 B 的套件檔。
此操作完成之後，會多佔用 10.2 kB 的磁碟空間。
下載:1 http://tw.archive.ubuntu.com/ubuntu focal/main amd64 python-is-python3 all 3.8.2-4 [2,364 B]
取得 2,364 B 用了 0s (9,664 B/s)             
選取了原先未選的套件 python-is-python3。
（讀取資料庫 ... 目前共安裝了 246237 個檔案和目錄。）
正在準備解包 .../python-is-python3_3.8.2-4_all.deb……
解開 python-is-python3 (3.8.2-4) 中...
設定 python-is-python3 (3.8.2-4) ...
```
這時再執行一次 `python`
```bash
$ python
Python 3.8.10 (default, Sep 28 2021, 16:10:42) 
[GCC 9.3.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> 
```
還原只要移除 `python-is-python3`，移除指令如下
```bash
$ sudo apt remove python-is-python3
```

## 參考文章
1. [sudo apt install python-is-python3](https://gist.github.com/takurx/57342618e21b3c1afdf841956cc23da4)
