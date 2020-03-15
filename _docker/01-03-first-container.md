---
title: "1.3 建立第一個軟體貨櫃 (Application Container)"
permalink: /docker/first-container/
excerpt: "在安裝 Docker CE[^get_docker_ce]之前，必需先在系統上安裝 Docker 的環境，這裡示範在 Ubuntu Linux 中的安裝步驟。"
last_modified_at: 2019-12-23T21:36:18-23:00
toc: true
categories:
  - 虛擬化
tags:
  - docker
  - container
---

[上一篇]()

## 安裝 Docker CE
在安裝 Docker CE[^get_docker_ce]之前，必需先在系統上安裝 Docker 的環境，這裡示範在 Ubuntu Linux 中的安裝步驟。

[^get_docker_ce]: [安裝 Docker CE 的官方文件](https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/#install-using-the-repository)

### 事前準備

安裝基本套件 :
```bash
$ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
```

加入 Docker 的 GPG 金鑰
```bash
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

加入 Docker 套件庫
```bash
$ sudo add-apt-repository "deb [arch=amd64]  https://download.docker.com/linux/ubuntu  $(lsb_release -cs) stable"
```

### 開始安裝

更新 Ubuntu Linux 套件清單
```bash
$ sudo apt update
```

安裝 Docker CE（Community Edition）
```bash
$ sudo apt install docker-ce
```

**加入 docker 群組**

如果要以一般使用者的權限執行Docker，就要將使用者加入`docker`群組內，這樣就不需使用 `sudo` 命令執行 Docker。
```bash
$ sudo usermod -aG docker $USER
```

**注意:** 如果沒有把使用者加入 `docker` 群組的話，就會出現這樣的錯誤訊息：
ERROR: Got <font color="red">permission denied</font> while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get http://%2Fvar%2Frun%2Fdocker.sock/v1.40/info: dial unix /var/run/docker.sock: connect: permission denied
errors pretty printing info
{: .notice--danger}

重新開機 <font color="red">(一定要執行)</font>
```bash
$ sudo reboot
```

### 檢視 Docker 運作架構資訊
```bash
$ docker info
```

<figure>
  <img src="{{ '/assets/images/02-9-docker-info.png' | relative_url }}" alt="Docker 運作架構資訊">
</figure>

**重要:** 在 17.06 以後的版本 overlay2 檔案系統的安裝順序在 Aufs 之前，
Docker 選擇 Storage Driver 的順序如下 : zfs --> overlay2 --> aufs --> overlay --> devicemapper --> vfs
{: .notice--danger}

### 檢查 Docker 是否能正常運行
```bash
$ sudo docker run hello-world
```
因為沒有下載過hello-world這個影像檔，所以會先下載，正常結果如下圖
<figure>
  <img src="{{ '/assets/images/02-10-docker-hello-world.png' | relative_url }}" alt="Docker hello-world 輸出訊息">
  <figcaption>Docker hello-world 輸出訊息</figcaption>
</figure>

---
**參考文章**