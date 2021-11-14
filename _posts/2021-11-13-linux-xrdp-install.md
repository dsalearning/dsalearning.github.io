---
title: "在 Ubuntu 20.04 安裝 Xrdp 就可以用遠端桌面連線"
permalink: /linux/linux-xrdp-install/
excerpt: "用 Windows 的遠端桌面也可以連線到 Linux ! 只要在 Linux 系統安裝 Xrdp 就可以嘍。"
header:
  teaser: /assets/images/linux/xrdp-on-ubuntu.png
last_modified_at: 2021-11-13T23:00:00-00:00
toc: true
categories:
  - Linux
tags:
  - 遠端桌面
  - Xrdp
# sidebar:
#   nav: "linux"
author_profile: true
---

<figure class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/linux/xrdp-on-ubuntu.png" alt="">
</figure> 

## 作業環境
* Ubuntu 20.04.3 LTS
* Windows 10 專業版

## 前置需求
在 `Ubuntu 20.04` 或 `Ubuntu 18.04` 已經安裝了桌面(Desktop)了，如果還沒安裝，則請先執行下列指令來安裝
```bash
$ sudo apt install ubuntu-desktop
```

## 安裝步驟
### Step 1：安裝 Xrdp
打開終端機(terminal)，輸入下列指令安裝 **Xrdp**
```bash
$ sudo apt install xrdp
```
當有提示問 Do you want to continue? [Y/n] 時，就輸入 `Y` 繼續安裝，安裝完之後， **Xrdp** 服務自動會啟用，你可以輸入下列指令確認。
```bash
$ sudo systemctl status xrdp
```
結果如下
```bash
● xrdp.service - xrdp daemon
     Loaded: loaded (/lib/systemd/system/xrdp.service; enabled; vendor preset: >
     Active: active (running) since Sat 2021-11-13 22:09:33 CST; 34min ago
       Docs: man:xrdp(8)
             man:xrdp.ini(5)
   Main PID: 867 (xrdp)
      Tasks: 2 (limit: 9368)
     Memory: 19.8M
     CGroup: /system.slice/xrdp.service
             ├─ 867 /usr/sbin/xrdp
             └─4561 /usr/sbin/xrdp
```

### Step 2：Xrdp 加入 ssl-cert 群組
當 **Xrdp** 安裝之後，有一個 `ssl-cert-snakeoil.key` 放置在 `/etc/ssl/private/` 目錄下，查看一下。
```bash
# ll /etc/ssl/private/
總用量 12
drwx--x--- 2 root ssl-cert 4096 11月 22  2019 ./
drwxr-xr-x 4 root root     4096 10月 15 22:05 ../
-rw-r----- 1 root ssl-cert 1704 11月 22  2019 ssl-cert-snakeoil.key
```
需要將 `xrdp` 使用者加到 `ssl-cert` 群組中，使得該文件可以被讀取，加入該群組的指令如下。
```bash
$ sudo adduser xrdp ssl-cert
```
結果如下
```bash
正將 `xrdp' 使用者新增至 `ssl-cert' 群組 ...
正在將 xrdp 使用者加入到 ssl-cert 群組
完成。
```
驗證是否有加入到該群組
```bash
$ cat /etc/group | grep xrdp
ssl-cert:x:110:xrdp
xrdp:x:128:
```

### Step 3：開通防火牆 3389 埠
如果有啟用防火牆， **Xrdp** 使用的埠號同 windows 一樣都是 `3389` 埠，下列指令是允許我本機這個子網段都可以遠端到此 `Ubuntu` 系統。
```bash
$ sudo ufw allow from 192.168.1.0/24 to any port 3389
```
設定好之後，刷新一下防火牆，並查看一下狀態
```bash
$ sudo ufw reload
$ sudo ufw status
```
結果如下
```bash
狀態： 啓用

至                          動作         來自
--                          --          --     
3389                       ALLOW       192.168.1.0/24     
```
筆者試了一台有啟用防火牆，依上述即可遠端到 `Ubuntu`，另一台沒有啟用防火牆，與 `Windows 10` 主機是同網段，一樣是可以遠端到 `Ubuntu`。

### Step 4：遠端桌面到 Ubuntu 桌面
回到 `Windows 10` 系統，鍵盤按 [Windows](#link){: .btn .btn--primary} + [R](#link){: .btn .btn--primary} 鍵，輸入 `mstsc` 即可開啟**遠端桌面連線**，或是 [開始](#link){: .btn .btn--inverse} > [Windows 附屬應用程式](#link){: .btn .btn--inverse} > [遠端桌面連線](#link){: .btn .btn--inverse} 開啟。

<img src="{{ '/assets/images/linux/xrdp-rdp-client.png' | relative_url }}" alt="">

上圖點擊 [連線(N)](#link){: .btn .btn--primary}，會跳出下圖，請點擊[是(Y)](#link){: .btn .btn--primary}

<img src="{{ '/assets/images/linux/xrdp-rdp-client-verify.png' | relative_url }}" alt="">

正常的情況下會出現下面左圖的登入畫面，輸入 `Ubuntu` 系統的使用者帳密，然後按 [OK](#link){: .btn .btn--primary}，接著是出現下面右圖在 `Ubuntu` 系統的驗證，再次輸入登入的密碼。

<figure class="half">
  <a href="/assets/images/linux/xrdp-rdp-client-login.png"><img src="/assets/images/linux/xrdp-rdp-client-login.png"></a>
  <a href="/assets/images/linux/xrdp-rdp-client-autuentication.png"><img src="/assets/images/linux/xrdp-rdp-client-autuentication.png"></a>
</figure>

但筆者也有一台遇到了**黑屏**的問題。

## 黑屏畫面問題處理
使用文字編輯器開啟 `/etc/xrdp/startwm.sh`，筆者慣用 nano 編輯，或者可以用 vim 編輯。
```bash
$ sudo nano /etc/xrdp/startwm.sh
```
將下列二行加在在 test 和 exec `Xsession` 之前。
```bash
unset DBUS_SESSION_BUS_ADDRESS
unset XDG_RUNTIME_DIR
```
例如
```bash
#!/bin/sh
# xrdp X session start script (c) 2015, 2017 mirabilos
# published under The MirOS Licence

if test -r /etc/profile; then
        . /etc/profile
fi
unset DBUS_SESSION_BUS_ADDRESS
unset XDG_RUNTIME_DIR

test -x /etc/X11/Xsession && exec /etc/X11/Xsession
exec /bin/sh /etc/X11/Xsession
```
編輯好後存檔，然後重啟 **Xrdp** 服務，重啟指令如下。
```bash
$ sudo systemctl restart xrdp
```
當黑屏問題解決掉後，重新用遠端桌面登入，之後就可看到下列的畫面了。
<figure class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/linux/xrdp-rdp-client-ubuntu-desktop.png" alt="">
</figure> 

## 雙螢幕推薦
如果你是使用雙螢幕，在開啟遠端桌面連線時，到 [顯示](#link){: .btn .btn--inverse} > [遠端工作階段使用我的所有監視器(U)](#link){: .btn .btn--inverse} 勾選起來。

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/linux/xrdp-rdp-client-monitor.png" alt="">

就可以體驗雙螢幕操作的爽度，推薦一定要使用雙螢幕。
<figure class="align-center">
  <a href="/assets/images/linux/xrdp-rdp-client-ubuntu-desktop2.png"><img src="{{ site.url }}{{ site.baseurl }}/assets/images/linux/xrdp-rdp-client-ubuntu-desktop2.png" alt=""></a>
</figure> 
## 參考文章
1. [How to Install Xrdp on Ubuntu 20.04](https://www.tecmint.com/install-xrdp-on-ubuntu/)
