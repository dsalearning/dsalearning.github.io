---
title:  "樹莓派安裝Jupyter Notebook"
excerpt: "Jupyter Notebook對於熟悉Python與數據分析的人員來說是必備的工具，一般都是在安裝電腦上，而樹莓派也支援這個工具的安裝！"
header:
  teaser: assets/images/raspberry-pi-4-model-b.jpg
sidebar:
  - title: "進修課程名稱"
    text: "物聯網樹莓派通訊程式與影像運用實務班"
  - title: "進修日期"
    text: "2020/03/14~2020/05/23"
  - title: "講師"
    text: "徐國堂"
search: false
categories: 
  - AIoT
tags:
  - Raspberry Pi
  - 樹莓派
  - Raspbian
  - 物聯網
  - 安裝
last_modified_at: 2020-03-22T21:00-00:00
toc: true
---

## 安裝Jupyter Notebook

```bash
$ sudo pip3 install jupyter
```

## 讓本機的電腦可以連樹莓派的Jupyter Notebook

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

你可以輸入下列命令檢查該 `.py` 檔的權限

```bash
$ ls -al
```

修改 `.py` 檔的c.NotebookApp.ip，在終端機輸入下列命令

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


## 心得
在安裝Jupyter Notebook後，可以正常啟動，但是執行時並沒有把執行結果輸出到畫面。

## 參考文章 ##
