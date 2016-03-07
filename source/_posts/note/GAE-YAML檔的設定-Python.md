---
title: 'GAE::YAML檔的設定(Python)'
category:
  - Note
date: 2014-06-07 10:12:55
tags:
- GAE
- YAML
- Python
---

上傳Python code上Google App Engine 時， YAML檔的一些小設定。

<!--more-->

``` yaml
application: mikuwebapp
version: 1
runtime: python27
api_version: 1
threadsafe: true

handlers:
- url: /.*
  script: mikuwebapp.app
```

這是我的 yaml，第一行application，是你的程式網址，也就是 X.appspot.com的X那部分。
version，程式版本，如果跟之前上傳的不一樣，Google會幫你把之前版本的被分留著，可以復原。
runtime，跑的環境。一般好像預設是Python 2.5，我設定 python27，是GAE支援的最新Python版本。
api_version，這個我不太清楚，好像就是api 的版本。
threadsafe: true，如果設定Python，就要加這行。


handlers: - url: /.*，這裡說的是，收到網址後要交給哪支程式執行他，後面是正則表達式，這裡 /.* 代表所有的URL

script: mikuwebapp.app，這裡是要使用的Python程式名稱+.app，注意使用 Python27時，附檔名要改成app或是像Google上講的 application，而且前面一定要有**兩個空白**，Python 的空白是會影響程式運行的。

至於為甚麼附檔名不是py是app，那是因為我們的Python程式裡有一行呼叫我們的class執行的程式碼

``` python
app = webapp2.WSGIApplication([('/', MainPage),], debug=True)
```

app就是這裡的app，所以基本上只要一樣，取什麼都可。

順便解釋這行，('/', MainPage)是只如果收到的路徑是 '/'，就呼叫MainPage class，這個類我們要在上面自己定義，並且要包含兩個函式 get 和 post ，缺少的話，使用者使用該功能會失敗(但是不會影響程式執行)，依此類推我們可以定義多個class和路徑指定。

要注意的重點是，如果網頁最後跑不出東西，八成是Python code爛掉，注意該加的逗號加了沒，還有**空白**加了沒(每個縮牌要有至少**兩個空白**，GAE上的Python**不能用 TAB**)。

對了對了，如果Python裡有用到中文，要記得在第一行加上 **#coding=utf-8 **!!!!!
