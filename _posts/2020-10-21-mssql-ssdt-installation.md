---
title:  "MSSQL-如何離線安裝 Reporting Services 2016 的開發工具？"
excerpt: "SQL Server Data Tools (SSDT) 是一款新式開發工具，舉凡 SSAS、SSIS 與 SSRS 都可以用這個工具來建置。"
header:
  teaser: assets/images/mssql/ssdt-installation.png
search: false
categories: 
  - Databases
tags:
  - SSDT
  - SSRS  
  - MSSQL
last_modified_at: 2020-10-20T15:00-00:00
toc: true
---

有時候安裝工具沒有想像中的順利，就像筆者遇到的問題，要在一台不能連上網際網路的電腦安裝 Reporting Services (RS) 2016 的開發工具，以這個版本在當期可以用 Visual Studio 2017 與 Visual Studio 2019 來開發，但必需另外安裝 SQL Server Data Tools (以下簡稱 SSDT) 。

SSDT 是一款新式開發工具，可用來建置 SQL Server 關聯式資料庫、Azure SQL 中的資料庫、Analysis Services (AS) 資料模型、Integration Services (IS) 套件和 Reporting Services (RS) 報表。 有了 SSDT，您便可設計和部署任何 SQL Server 內容類型，就像在 Visual Studio 中開發應用程式一樣容易。

**注意** 沒有適用於 Visual Studio 2019 的 SSDT 獨立安裝程式
{: .notice--danger}

所以接下來我們用Visual Studio 2017 的 SSDT 離線安裝吧。

## 離線安裝

首先在一台能連上網際網路的電腦完成下列步驟：
1. [下載 SSDT 的獨立安裝程式。](https://docs.microsoft.com/zh-tw/sql/ssdt/download-sql-server-data-tools-ssdt?view=sql-server-2016#ssdt-for-vs-2017-standalone-installer)
2. [下載 vs_sql.exe](https://aka.ms/vs/15/release/vs_sql.exe)

先來查看剛下載的檔案
```bash
C:\Users\alvinlin> D:
D:\> cd temp
D:\Temp> dir
 磁碟區 D 中的磁碟是 DATA
 磁碟區序號:  CA1A-0053

 D:\Temp 的目錄

2020/10/20  上午 11:08    <DIR>          .
2020/10/20  上午 11:08    <DIR>          ..
2020/10/20  上午 11:07       949,585,768 SSDT-Setup-CHT.exe
2020/10/20  上午 11:08         1,265,408 vs_SQL.exe
               2 個檔案     950,851,176 位元組
               2 個目錄  260,921,925,632 位元組可用
```
**注意** 筆者當時的作業環境是在繁中，所以下載下來的 SSDT 安裝程式是 `SSDT-Setup-CHT.exe`
{: .notice--danger}

當這二個檔案都下載完成後，這裡筆者是下載到 `D:\Temp` ，接下來執行下列命令，才能真正把離線安裝的檔案全部下載下來。
```bash
D:\Temp> vs_sql.exe --layout D:\Temp\vs2017ssdt
```

使用 `--layout` 選項是關鍵，這會為離線安裝下載實際檔案。 以實際配置路徑取代 `<filepath>` 來儲存檔案。
* 針對特定語言，請傳遞地區設定：`vs_sql.exe --layout c:\<filepath> --lang en-us` (一種語言 ~1 GB)。
* 針對所有語言，請省略 `--lang` 引數：`vs_sql.exe --layout c:\<filepath>` (所有語言 ~3.9 GB)。

完成前述步驟之後，就可以執行下列步驟進行離線安裝：

* #1 執行 `vs_setup.exe --NoWeb`，以安裝 VS2017 Shell 及 SQL Server 資料專案。
  依照慣例，我們先檢查一下再執行

  ```bash
  D:\Temp\vs2017ssdt> dir vs_s*.exe
  磁碟區 D 中的磁碟是 DATA
  磁碟區序號:  CA1A-0053

  D:\Temp\vs2017ssdt 的目錄

  2020/10/20  上午 11:08         1,265,408 vs_setup.exe
  2020/10/20  上午 11:08         1,265,408 vs_SQL.exe
                2 個檔案       2,530,816 位元組
                0 個目錄  260,921,450,496 位元組可用

  D:\Temp\vs2017> vs_setup.exe --NoWeb
  ```

  **注意** 筆者當時安裝時，遇到安裝失敗，經查看 Log 後，發現是 `vc_redist.x64.exe` 安裝失敗，因為筆者沒有先了解[系統需求](https://docs.microsoft.com/zh-tw/visualstudio/productinfo/vs2017-system-requirements-vs)，所以安裝前請務必確保 `vc_redist.x64.exe` 已先安裝，請參考[最新支援的 Visual C++ 下載](https://support.microsoft.com/zh-tw/help/2977003/the-latest-supported-visual-c-downloads)。
  {: .notice--danger}  

* #2 從配置資料夾執行 `SSDT-Setup-ENU.exe /install`，並選取 SSIS/SSRS/SSAS。 a. 若要自動安裝，請執行 `SSDT-Setup-ENU.exe /INSTALLALL[:vsinstances] /passive`。
  這裡我採用選取的方式

  ```bash
  D:\Temp\vs2017> SSDT-Setup-CHT.exe /install
  ```
  正常會出現如下圖
  <figure class="align-center">
    <img src="{{ site.url }}{{ site.baseurl }}/assets/images/mssql/ssdt-installation.png" alt="">
  </figure> 

  有沒有發現筆者上述 #2 用的是 `SSDT-Setup-CHT.exe`，結果因為語系不同，導致安裝失敗了，當時是在簡中的環境執行 `SSDT-Setup-CHS.exe /install`，結果出現如下圖的錯誤。
  <figure class="align-center">
    <img src="{{ site.url }}{{ site.baseurl }}/assets/images/mssql/ssdt-setup-lang-error.png" alt="">
  </figure> 

  所以，請選擇[可用語言 - 適用於 VS 2017 的 SSDT](https://docs.microsoft.com/zh-tw/sql/ssdt/download-sql-server-data-tools-ssdt?view=sql-server-2016#available-languages---ssdt-for-vs-2017)
  * [英文 (美國)](https://go.microsoft.com/fwlink/?linkid=2139376&clcid=0x409)

  由於筆者要安裝的環境是英文環境，所以改用英文版的吧!

  在改用英文版安裝時，再度發生了小插曲，在安裝完 SSRS 還是 `vc_redist.x64.exe` 後，似乎有要求要重開機，筆者當時沒有注意就取消，結果就出現下圖。
  <figure class="align-center">
    <img src="{{ site.url }}{{ site.baseurl }}/assets/images/mssql/ssdt-setup-reboot-error.png" alt="">
  </figure> 

  好吧! 那就重開機再繼續剛才的指令，終於安裝成功了。
  <figure class="align-center">
    <img src="{{ site.url }}{{ site.baseurl }}/assets/images/mssql/ssdt-setup-completed.png" alt="">
  </figure> 

若要查看可用的選項，請執行 `SSDT-Setup-ENU.exe /help`

## 心得
* 安裝前先檢查系統安裝的要求，否則安裝失敗時再去看 Log 更花時間。
* 當有訊息跳出時，還是要仔細看一下，不要慣性的「下一步」。


## 參考文章
1. [下載適用於 Visual Studio 的 SQL Server Data Tools (SSDT)](https://docs.microsoft.com/zh-tw/sql/ssdt/download-sql-server-data-tools-ssdt?view=sql-server-2016)