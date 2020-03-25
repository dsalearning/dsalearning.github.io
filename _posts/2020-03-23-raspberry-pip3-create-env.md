---
title:  "在樹莓派建立Python虛擬環境"
excerpt: "在本機電腦都用 conda 來建立虛擬環境，且可以用它的 IDE 建立，但 conda 太肥了，在樹莓派裡就用 Python 的指令來建立嘍。"
header:
  teaser: assets/images/raspberry-pi-python-create-env.png
search: false
categories: 
  - AIoT
tags:
  - Raspberry Pi
  - 樹莓派
  - Python
  - 物聯網
  - 虛擬環境
last_modified_at: 2020-03-23T21:00-00:00
toc: true
---
根據筆者接觸 Python 的少少經驗來說，就已經吃了不少虧，都是因為運行的程式版本的差異導致異常，因此為了讓程式能運作正常，建立虛擬環境也就變得是必要的，在樹莓派建虛擬環境並不是用 Anaconda 來建立， 以樹莓派這樣的硬體等級就讓我打消安裝 Anaconda，還是輕量化的安裝就好，沒有必要安裝一堆你用不到的物件。

## 前置準備
* 建立安裝的路徑，之後可能會有多個虛擬環境，所以我們集中在 `~/Documents/envs` 的這個目錄下。
```bash
$ cd ~/Documents
$ sudo mkdir envs
```
* 檢視一下建立的目錄與權限
```bash
$ ls -al
```
結果如下
```
pi@raspberrypi:~/Documents $ ls -al
total 12
drwxr-xr-x  3 pi   pi   4096 Mar 25 23:01 .
drwxr-xr-x 18 pi   pi   4096 Mar 25 22:57 ..
drwxr-xr-x  2 root root 4096 Mar 25 23:01 envs
```

## 開始安裝 
* 切換到剛建好的目錄 `envs`
```bash
$ cd envs
```
* 更新 `pip`
```bash
$ pip3 install --upgrade pip
```
結果如下
```
pi@raspberrypi:~/Documents/envs $ pip3 install --upgrade pip
Looking in indexes: https://pypi.org/simple, https://www.piwheels.org/simple
Collecting pip
  Downloading https://files.pythonhosted.org/packages/54/0c/d01aa759fdc501a58f431eb594a17495f15b88da142ce14b5845662c13f3/pip-20.0.2-py2.py3-none-any.whl (1.4MB)
    100% |████████████████████████████████| 1.4MB 257kB/s 
Installing collected packages: pip
Successfully installed pip-20.0.2
```
* 檢查是否有 `virtualenv` 程式
```bash
$ pip3 list
```
結果如下
```
pi@raspberrypi:~/Documents/envs $ pip3 list
WARNING: pip is being invoked by an old script wrapper. This will fail in a future version of pip.
Please see https://github.com/pypa/pip/issues/5599 for advice on fixing the underlying issue.
To avoid this problem you can invoke Python with '-m pip' instead of running pip directly.
Package           Version    
----------------- -----------
appdirs           1.4.3      
... 以下省略
urllib3           1.24.1     
wcwidth           0.1.7      
webencodings      0.5.1      
Werkzeug          0.14.1     
wheel             0.32.3     
wrapt             1.10.11    
```
* (錯誤) 安裝 `virtualenv` 程式，若上述有看過該程式則可以略過此步驟。
```bash
$ pip3 install virtualenv
```

**注意：**筆者一開始沒有用系統管理員 `sudo` 去建立，結果就出現了 `WARNING` ，之後導致無法建立虛擬環境，所以一定要用 系統管理員 `sudo` 建立。
{: .notice--info}
結果如下
```
pi@raspberrypi:~/Documents/envs $ pip3 install virtualenv
WARNING: pip is being invoked by an old script wrapper. This will fail in a future version of pip.
...以下省略
  WARNING: The script virtualenv is installed in '/home/pi/.local/bin' which is not on PATH.
  Consider adding this directory to PATH or, if you prefer to suppress this warning, use --no-warn-script-location.
Successfully installed distlib-0.3.0 filelock-3.0.12 importlib-metadata-1.5.0 virtualenv-20.0.14 zipp-3.1.0
```
這時你用 `pip3 list` 檢查會發現 `virtualenv` 是存在的，但是執行時會無法建立，會出現下列錯誤。
```bash
pi@raspberrypi:~/Documents/envs $ virtualenv dsalearning
bash: virtualenv: command not found
```

* (正確) 安裝 `virtualenv` 程式，若上述沒有看到 `WARNING` ，可能你的環境變數已有加了該路徑了吧，正常者可以略過此步驟。
```bash
$ sudo pip3 install virtualenv
```
結果如下
```
pi@raspberrypi:~/Documents/envs $ sudo pip3 install virtualenv
Looking in indexes: https://pypi.org/simple, https://www.piwheels.org/simple
Collecting virtualenv
...以下省略
Successfully installed distlib-0.3.0 filelock-3.0.12 importlib-metadata-1.5.0 virtualenv-20.0.14 zipp-3.1.0
```
耶！沒有看到 `WARNING` 了，來建立虛擬環境吧。
```bash
$ sudo virtualenv dsalearning
```
結果如下
```
pi@raspberrypi:~/Documents/envs $ sudo virtualenv dsalearning
created virtual environment CPython3.7.3.final.0-32 in 407ms
  creator CPython3Posix(dest=/home/pi/Documents/envs/dsalearning, clear=False, global=False)
  seeder FromAppData(download=False, pip=latest, setuptools=latest, wheel=latest, via=copy, app_data_dir=/root/.local/share/virtualenv/seed-app-data/v1.0.1)
  activators BashActivator,CShellActivator,FishActivator,PowerShellActivator,PythonActivator,XonshActivator
```
## 檢查虛擬環境
* 檢查目錄
```bash
$ ls -al
```
結果如下
```
pi@raspberrypi:~/Documents/envs $ ls -al
total 12
drwxr-xr-x 3 root root 4096 Mar 26 00:18 .
drwxr-xr-x 3 pi   pi   4096 Mar 25 23:01 ..
drwxr-xr-x 4 root root 4096 Mar 26 00:18 dsalearning
```

## 進入虛擬環境
```bash
$ cd dsalearning
$ ls -al
$ source bin/activate
```
結果如下
```
pi@raspberrypi:~/Documents/envs/dsalearning $ ls -al
total 20
drwxr-xr-x 4 root root 4096 Mar 26 00:18 .
drwxr-xr-x 3 root root 4096 Mar 26 00:18 ..
drwxr-xr-x 2 root root 4096 Mar 26 00:18 bin
drwxr-xr-x 3 root root 4096 Mar 26 00:18 lib
-rw-r--r-- 1 root root  202 Mar 26 00:18 pyvenv.cfg
pi@raspberrypi:~/Documents/envs/dsalearning $ source bin/activate
(dsalearning) pi@raspberrypi:~/Documents/envs/dsalearning $ 
```


## 檢查虛擬環境安裝的程式
```bash
$ pip3 list
```
結果如下
```
(dsalearning) pi@raspberrypi:~/Documents/envs/dsalearning $ pip3 list
Package    Version
---------- -------
pip        20.0.2 
setuptools 46.1.1 
wheel      0.34.2 
```
## 離開虛擬環境
```bash
$ deactivate
```


## 心得

## 參考文章
1.[Virtual Environments in Python Made Easy](https://www.sitepoint.com/virtual-environments-python-made-easy/)
