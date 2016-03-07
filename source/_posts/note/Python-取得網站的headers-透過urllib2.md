---
title: 'Python::取得網站的headers 透過urllib2'
category:
  - Note
date: 2014-08-07 10:24:35
tags:
- Python
---


在上udacity CS253的時候，有用到urllib2取得headers，可是照他說的做沒有成功......

後來查了一查，總結就是..

<!--more-->

``` Python
p = urllib2.urlopen('http://.....')
c = p.info().dict
print c['header name']
```

這樣， c 就是 headers 的字典物件(dict)，可以直接用 .item 取得所有的header或是用 ['字串']來取得。

附帶一提，函數  dir()  還不錯用啊......

dir(物件實例)  可以直接列出物件的方法。
