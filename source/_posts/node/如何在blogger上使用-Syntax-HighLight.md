---
title: 如何在blogger上使用 Syntax HighLight
category:
  - Node
date: 2014-05-23 07:21:34
tags:
- HTML&CSS
---

我之前在這裡貼code都是用其他的網站在連進來，雖然有點麻煩，但是比較好看。最近卻收到通知，該網站要開始收費了，於是我就重新思考要怎麼使用HighLight。

<!--more-->

後來Google了一下，找到的方法都很麻煩或是不易更新維護，但是這一篇 ([傳送門](http://goofyz.30sparks.com/2012/08/another-syntax-highlight-highlightjs.html))裡面講了一個好方法。

[highlightjs](https://highlightjs.org/)

說明就算了，這裡記錄一下用法，以免自己忘記。

首先到下面的網站，把 < 和 > 換成 &lt; 和 &gt;，如果有用Tab鍵建議取代成4個Space。
http://www.htmlescape.net/htmlescape_tool.html

再來到Blogger的設定->範本->編輯HTML
在`<head>`裡貼上

``` html
<link rel="stylesheet" href="http://yandex.st/highlightjs/7.0/styles/default.min.css" />
<script src="http://yandex.st/highlightjs/7.0/highlight.min.js"></script>
<script type="'text/javascript'">
hljs.initHighlightingOnLoad();
</script> 
```

分別是用來取代code的JS和CSS，CSS的部分還有很多選擇([傳送門](https://highlightjs.org/static/demo/))，改link就可以換CSS。

只要把想用的code 用 `<pre><code></code><pre>`包起來即可。

雖然他會自己判斷語言，不過因為有些原因她很容易判錯，可以先在code tag裡加上 class="cpp"  來指定語言，也可以其他種語言，細節官網上有。

如果想要換顏色，可以用瀏覽器找出該文字的class，把CSS下載下來並加上 pre .classname{顏色}
如果那段文字不屬於任何class，那就必須去改JS了，這比較麻煩。

總之，終於得到夢寐以求的 monokai 了!!!!(跟sublime類似的HighLight)。
對了，monokkai sublime  反而不像sublime，而且不知道為甚麼連不到。
於是我自己修了一點，並放在自己的Google drive上。


### 2016/01/26 小筆記

這篇是以前還在用Blogger的時候寫的，現在換到Hexo雖然一開始就有Highlight而且不錯看，但是說不定哪天我又會去換顏色~~。END
