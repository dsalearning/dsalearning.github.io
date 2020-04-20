---
title:  "Windows檔案批量更名，就是這麼簡單!"
excerpt: "在Windows更名一個檔案只要按 F2 ，但如果要一次更改多個檔名時，一個個改可真的很浪費時間，所以懂得一些命令是可以提升效率的喔。"
header:
  teaser: assets/images/windows/keyboard.png
search: false
categories: 
  - Windows
tags:
  - PowerShell
  - 小技巧
  - CMD
last_modified_at: 2020-04-20T12:00-13:00
toc: true
---
你是否也遇過這樣的案例，需要將一堆的檔案變更副檔名或是將檔名某字串取代成指定的呢? 若檔案在十個以內，一個個改是還好，但檔案數超過十個百個以上時，難道這時也一個個改嗎! 應該有批量的修改方式吧! 是的，下列介紹怎麼批量修改這二種情境。

## 批量修改副檔名

請開啟**命令提示字元**，開啟的方式可以按[Windows](#link){: .btn .btn--primary} + [R](#link){: .btn .btn--primary}，再輸入 `cmd` ，或是在 `程式集` > `附屬應用程式` > `命令提示字元` 也可以找到。

筆者在工作時，會需要把 MSSQL 的 Script 匯出，但匯出時的名稱會跟命名規則不同，且這些程式數量蠻多的，不可能一個個改檔名，所以就需要會用 DOS Command 的指令來修改，這樣修改就很有效率，下面範例是把副檔名 `.sql` 變更為 `.txt`  。

**使用 `dir` 查看修改的目錄檔案**

為了能前後比對，先把要修改的檔案列出來

```
D:\Test>dir
 磁碟區 D 中的磁碟是 DATA
 磁碟區序號:  CA1A-0053

 D:\Test 的目錄

2020/04/20  下午 12:02    <DIR>          .
2020/04/20  下午 12:02    <DIR>          ..
2020/04/20  上午 10:39                 0 dbo.uvTest (1).View.sql
2020/04/20  上午 10:39                 0 dbo.uvTest (2).View.sql
2020/04/20  上午 10:39                 0 dbo.uvTest (3).View.sql
2020/04/20  上午 10:39                 0 dbo.uvTest (4).View.sql
2020/04/20  上午 10:39                 0 dbo.uvTest (5).View.sql
              12 個檔案               0 位元組
               2 個目錄  270,111,956,992 位元組可用
```
**使用 `ren` 或 `rename` 指令更改副檔名**

將上面副檔名是 `.sql` 的檔案更改為副檔名是 `.txt`，修改的命令如下:
```powershell
> rename *.sql *.txt
```
再用 `dir` 查詢一次，可以看到變更了。
```
D:\Test>dir
 磁碟區 D 中的磁碟是 DATA
 磁碟區序號:  CA1A-0053

 D:\Test 的目錄

2020/04/20  下午 01:12    <DIR>          .
2020/04/20  下午 01:12    <DIR>          ..
2020/04/20  上午 10:39                 0 dbo.uvTest (1).View.txt
2020/04/20  上午 10:39                 0 dbo.uvTest (2).View.txt
2020/04/20  上午 10:39                 0 dbo.uvTest (3).View.txt
2020/04/20  上午 10:39                 0 dbo.uvTest (4).View.txt
2020/04/20  上午 10:39                 0 dbo.uvTest (5).View.txt
               5 個檔案               0 位元組
               2 個目錄  270,111,895,552 位元組可用
```
不過 `.sql` 才是筆者要的，變更成 `.txt` 只是示範而已，所以再改回來吧。

## 批量檔名字串取代

在這 Test 資料夾的這些 `.sql` 檔會發現檔名最後有 `.View` ，這個不是筆者要的，那要怎麼一次性的變更呢? 如果用上述的 `ren` 命令只能從 `.sql` 變更成 `.View.sql` ，但無法從 `.View.sql` 變更成 `.sql` ，因為最後一個點的後面才是副檔名，那要如何把 `.View` 給去掉呢? 這裡我們使用 PowerShell 來修改。

請開啟 **PowerShell** ，開啟的方式可以按[Windows](#link){: .btn .btn--primary} + [R](#link){: .btn .btn--primary}，再輸入 `powershell` ，或是在 `程式集` > `系統管理工具` > `Windows PowerShell Modules` 也可以找到。

開啟後，切換到該目錄，先使用 `dir` 把所有的檔案列出來，然後交給 `Rename-Item` 指令進行更名，這裡是把 `.View` 取代成空白，修改的命令如下。
```powershell
> dir | Rename-Item -NewName { $_.name -replace ".View",""}
```
執行結果如下:
```
PS C:\Users\alvinlin> d:
PS D:\> cd d:\test
PS D:\test> dir | Rename-Item -NewName {$_.name -replace ".View",""}
PS D:\test> dir

    目錄: D:\test

Mode                LastWriteTime     Length Name
----                -------------     ------ ----
-a---       2020/4/20  上午 10:39          0 dbo.uvTest (1).txt
-a---       2020/4/20  上午 10:39          0 dbo.uvTest (2).txt
-a---       2020/4/20  上午 10:39          0 dbo.uvTest (3).txt
-a---       2020/4/20  上午 10:39          0 dbo.uvTest (4).txt
-a---       2020/4/20  上午 10:39          0 dbo.uvTest (5).txt

PS D:\test>
```
看吧! 就是這麼簡單，如果有子資料夾的話，應該可以利用迴圈的方式來修改，有機會再來進一步介紹及 `Rename-Item` 其它功能。