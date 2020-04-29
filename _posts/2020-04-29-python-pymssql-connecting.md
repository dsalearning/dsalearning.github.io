---
title:  "使用 pymssql 連線到 MSSQL"
excerpt: "Firebase 提供了 SDK 讓用戶端建構應用程式，能夠即時的存儲與同步資料，任何連接的設備都可以在幾毫秒內收到更新，這樣不同的設備也能彼此溝通，來看看怎麼安裝吧。"
header:
  teaser: assets/images/python/pymssql-connecting.png
search: false
categories: 
  - Programming
tags:
  - Python
  - MSSQL
last_modified_at: 2020-04-28T21:00-00:00
toc: true
---

最近同事申請 MSSQL 資料庫權限，說要用 Python 連接，但他們都用 pymssql 套件，後來跟我說連不上時，我就請他改用 SQLAlchemy 測試，也提供的連線的語句，之後撥空做了這個測試是可以連線的，可能當時連線字串有問題或防火牆還沒通吧！接下來就來看看怎麼用 python 連接到 MSSQL吧。

## 作業環境 ##
* Windows 7
* Anaconda3
* Python 3.7.4
* MSSQL 2008 R2

## 安裝 pymssql
筆者本機已安裝了 Anaconda3 ，所以開啟命令提示字元 (cmd) ，先進入之前建立好的虛擬環境，命令如下：
```bash
> conda activate dsalearning
```
如果你還不知道怎麼建立虛擬環境，可以上網查一下嘍，筆者有空時會再補上如何建立虛擬環境。

這時檢查一下是否已安裝 pymssql 套件，檢查命令如下：
```bash
> pip list
```
筆者的結果如下：
```
(dsalearning) C:\Users\alvinlin>pip list
Package      Version
------------ -------------------
certifi      2020.4.5.1
pip          20.0.2
setuptools   46.1.3.post20200330
wheel        0.34.2
wincertstore 0.2
```
還沒有安裝，輸入下列命令來安裝 pymssql 套件吧。
```bash
$ pip install pymssql
```
安裝的速度很快，回傳結果如下：
```
(dsalearning) C:\Users\alvinlin>pip install pymssql
Collecting pymssql
  Using cached pymssql-2.1.4-cp37-cp37m-win_amd64.whl (411 kB)
Installing collected packages: pymssql
Successfully installed pymssql-2.1.4
```
這時你可以再用檢查套件的命令查看一下。

## 連線 MSSQL
使用 pymssql 套件的 `connect()` 函式來連接到 MSSQL Database，並傳入 `host` 、 `user` 、 `password` 及 `database` 等四個參數，連接的命令如下。
```python
import pymssql
conn = pymssql.connect(
    server='yourserver'
    , user='yourusername'
    , password='yourpassword'
    , database='yourdatabase'
)
```
請自行替換這四個參數值

## 執行查詢
使用 conn.cursor() 函式取得 `cursor` 物件 ， cursor.execute() 函式可用來擷取對 SQL Database 查詢的結果集，cursor.fetchone() 反覆查詢結果集。
```python
import pymssql

conn = pymssql.connect(
    server='yourserver'
    , user='yourusername'
    , password='yourpassword'
    , database='yourdatabase'
)
cursor = conn.cursor()

sql="select top 10 * from [dbo].[tblA]"

cursor.execute(sql)
row = cursor.fetchone()
while row:
	print(str(row[0])+" "+str(row[1])+" "+str(row[2]))
	row = cursor.fetchone() 
conn.close()
```
回傳的結果如下圖
<figure class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/python/pymssql-connecting.png" alt="">
</figure> 



## 參考文章
1. [使用 pymssql 連線到 SQL 的概念證明](https://docs.microsoft.com/zh-tw/sql/connect/python/pymssql/step-3-proof-of-concept-connecting-to-sql-using-pymssql?view=sql-server-ver15)
2. [Example scripts using pymssql module](https://pythonhosted.org/pymssql/pymssql_examples.html)