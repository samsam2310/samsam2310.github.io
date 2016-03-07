---
title: 'GAE::yaml中設定樹狀目錄'
category:
  - Note
date: 2014-08-03 10:22:15
tags:
- YAML
- GAE
- Python
---

寫GAE的時候，想要把Python檔放在不同的子資料夾裡，但是yaml 檔不知道怎麼設定。

<!--more-->

在yaml裡，script 是被當作物件之類的東西呼叫的，所以如果某個balaba.py放在 infor資料夾下面，那我們應該這樣寫。

``` yaml
- url: /bababa.* #這裡隨自己喜歡的設定，不影響
  script: infor.balaba.app
```

注意 infor後面接的是 '.' 而不是 '\' 或 '/'，但只有這樣還不夠，因為Python會把那個資料夾當作類似物件的存在(這是個人的理解，有錯請指正)，所以，必須在infor這個資料夾下面，**新增一個 __init__.py 的Python檔**，這是Python物件的建構子，裡面的內容可以空白沒關係。

如果有多層資料夾就依此類推，在每一層放一個空的__init__.py。
