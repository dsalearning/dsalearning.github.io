---
title: "1.1 Docker簡介"
permalink: /docker/introduction/
excerpt: "很多人都覺得 Docker是個新技術，其實不然，Docker除了其開發語言用 go比較新外，其實它還真不是個新東西，也就是個新瓶裝舊酒的東西，所謂的 The New 「Old Stuff」。"
header:
  teaser: assets/images/docker/docker-1-1-instroduction-logo.png
last_modified_at: 2019-12-20T13:00:00-05:00
toc: true
categories:
  - 虛擬化
tags:
  - docker
  - container
sidebar:
  nav: "docker"
author_profile: false
---
很多人都覺得 Docker是個新技術，其實不然，[Docker](https://www.docker.com/) 除了其開發語言用 [go](https://golang.org/) 比較新外，其實它還真不是個新東西，也就是個新瓶裝舊酒的東西，所謂的 The New 「Old Stuff」。
## 認識 Docker 軟體貨櫃技術
### Docker 使用的 Linux 核心模組功能包括下列各項
* Namespace - 用來隔離不同 Container 的執行空間
* Cgroup - 用來分配硬體資源
* chroot - 針對正在運作的軟體行程和它的子行程，改變它外顯的根目錄
* Bridge (Network) - 建立虛擬網路，連接不同 Container 
* AUFS/Overlay2(chroot) - 用來建立不同 Container 的檔案系統

### Docker 安全防護
* Capability - 控制 root 帳戶的執行功能[^DockerSecurity]
* Seccomp - Linux内核的安全計算模式（Seccomp） 功能
* AppArmor - 保護 Container 的網路及執行安全
* SELinux - Linux 的安全性增強模組

[^DockerSecurity]: [Hardening Docker containers and hosts against vulnerabilities: a security toolkit](https://www.stackrox.com/post/2017/08/hardening-docker-containers-and-hosts-against-vulnerabilities-a-security-toolkit/) 

## Docker 軟體貨櫃運作架構
<figure>
  <img src="{{ '/assets/images/docker/01-4-docker-simple-architecture.png' | relative_url }}" alt="Docker軟體貨櫃運作架構">
</figure>
1. Docker Daemon 只負責 Container 的建立，啟動，停止，移除及監控
2. Container 運作架構，是由 Linux 早已存在的核心模組 (Namespace, Cgroup, ..) 組成
3. 所有 Container 因共用 Host OS 的系統核心，使之啟動與關閉非常快速

## 討論 
### 討論1
Container 的執行資源 (CPU, Memory, Disk, Network) 需透過 Docker Daemon 處理嗎 ?

**答：**不需要，所以 Docker 軟體貨櫃不是虛擬系統
{: .notice--success}

### 討論2
Container 所依附的 Host OS 可以是 Windows 或 MAC OS ?

**答：**可以的[^DockerOnWindowsToday]
{: .notice--success}

**Note:** 1) HyperKit currently only supports Mac OS X using the Hypervisor.framework. It is a core component of Docker For Mac. 2) Docker Toolbox : Available for both Windows and Mac, the Toolbox installs Docker Client, Machine, Compose and Kitematic (圖形管理工具).
{: .notice--info}

[^DockerOnWindowsToday]: [Docker on Windows Today (2017/04 的訊息)](http://thevarguy.com/open-source-application-software-companies/microsofts-docker-strategy-future-windows-containers)

---
## 參考文章

