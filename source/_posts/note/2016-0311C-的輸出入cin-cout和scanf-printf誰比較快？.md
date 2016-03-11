---
title: C++的輸出入cin/cout和scanf/printf誰比較快？
category:
  - Note
date: 2016-03-11 08:53:36
tags:
 - C++
---

有打過資訊競賽的人，一定有遇過用cin/cout結果TLE，換成scanf/printf就AC的情況。
難道cin/cout真的比較慢嗎？為什麼C++要做出一個比C還要更慢的輸入輸出介面呢？

我們來看看cin/cout的效率到底怎麼樣。

<!--more-->

以下都是個人的觀察，有錯的話請留言告知QAQ，本人很廢還請鞭小力一點。

## 開始前，cin/cout是什麼？

首先，我們先來看一下cin/cout和scanf/printf的差別，前者是物件，後者是函數。
函數很簡單，就是定義一個函數，然後他會把裡面出現%的地方取代掉，而物件則是重載了shift運算子<<,>>，其實真的很直觀，就丟進cout跟從cin拿出來嘛～，而且也不用管型別，因為編譯器會幫你找運算子規則。
這裡我們發現，型別是編譯器處理的，和執行時完全沒有關係(別再說cin/cout慢是因為要判斷行別了)，而且自由度更高，可以自己定義。

那cin/cout到底慢再哪裡呢？
我們先用time指令做個小實驗，在Ubuntu 14.04筆電對一個檔案寫入1e7的random整數，這裡的程式碼都是簡化的code。

## 開始實驗

``` c++
for(int i = 0; i < (int)1e7; i++){
    printf("%d\n",rand());
}
// vs
for(int i = 0; i < (int)1e7; i++){
    cout<<rand()<<endl;
}
```

實驗三次，printf的時間分別是，
1.760 s
2.677 s
1.865 s

看起來很優秀，那cout呢？
15.921 s
15.188 s
15.685 s

發生了什麼事？怎麼慢成這樣！

## 優化1：sync_with_stdio 函數：和stdio同步

我已經看到那些篤定cin/cout不好的人偷笑的表情了，但是事情別說的太早，我們先看一下C++ Reference對於cin/cout的說明，我們發現了一個函數：std::ios_base::sync_with_stdio(false)，他是這樣說的，
> Toggles on or off synchronization of all the iostream standard streams with their corresponding standard C streams if it is called before the program performs its first input or output operation.

> If called once an input or output operation has occurred, its effects are implementation-defined.

> By default, iostream objects and cstdio streams are synchronized (as if this function was called with true as argument).
> With stdio synchronization turned off, iostream standard stream objects may operate independently of the standard C streams (although they are not required to), and mixing operations may result in unexpectedly interleaved characters.

看起來，cin/cout預設必須要跟stdin/stdout同步，所以必須做額外的運算，注意要是關掉了，scanf/printf就不能用了（如果用了，而且跟cin/cout混用，可能會吃到奇怪的東西），那我們試著把他關掉看看。

``` c++
ios_base::sync_with_stdio(false);
for(int i = 0; i < (int)1e7; i++){
    cout<<rand()<<endl;
}
```
結果：
13.120 s
14.958 s
15.165 s


看起來變快了兩秒，甚至根本沒變快，還是很慢啊...（你看看，自己慢還怪scanf/printf拖慢你）
等等，我們還忘了一個東西，endl。

## 優化2：endl 和 flush 物件：cout的緩衝區優化

什麼是endl，他是一個定義好的物件，在cout上給cout換行用的，那他跟<<'\n'有什麼差別呢？
原來，cout用了一個類似優化的設計，叫作緩衝區（由作業系統實作），所有的輸出都會先進到緩衝區裡，直到緩衝區滿了才會清空緩衝區並把字串輸出到stdout之類的輸出串流，難怪沒有跟stdout同步會出錯。
而當一般人寫程式的時候，輸出當然希望程式會把東西印到螢幕上，但是如果緩衝區還沒滿，我們就看不到結果了！
怎麼辦呢？cout有一個物件叫作flush（用法跟endl一樣），做的事情就是強迫清空緩衝區，並輸出到串流。
但是為什麼平常出學C++的人都沒有打過flush呢？原因有幾個，一個是Windows8以前的Windows CMD會自動清空緩衝區（或是根本沒有QAQ），另外一個主要的原因就是，其實endl就是<<'\n'<<flush;，對，endl就是換行加上flush，也就是說，如果我們用endl的話，就會強迫每個數字都清空緩衝區，累積一定量再一起輸出對cout來說可以優化一些操作，而這樣就破壞了這個優化了，我們試著把endl拿掉試試看。

``` c++
ios_base::sync_with_stdio(false);
for(int i = 0; i < (int)1e7; i++){
    cout<<rand()<<'\n';
}
```
結果：
2.765 s
1.708 s
1.713 s

太震驚了，去掉了endl之後，cout的速度已經和printf差不多快了！！整整快了12秒！！
原來效率就是在這種情況下不見的，那為什麼要作endl這種物件呢？
我們看看下面的實驗。

附註，其實printf也是有緩衝區的，只是他預設是到滿了才會清空。平常在console可以看到輸出是因為OS幫忙我們把緩衝區清掉了

## 優化3：cin.tie(0)：cin和cout綁定

我們先吃一個數字進來，再把他輸出。

``` c++
for(int i = 0; i < (int)1e7; i++){
    scanf("%d\n",&a);
    printf("%d\n",a+1); //output a+1;
}
// vs
for(int i = 0; i < (int)1e7; i++){
    cin>>a;
    cout<<a+1<<endl;
}
```

scanf/printf的時間：
2.579 s
3.994 s
3.241 s

而cin/cout：
19.970 s

不意外，那加上關閉同步的話？
``` c++
ios_base::sync_with_stdio(false);
for(int i = 0; i < (int)1e7; i++){
    cin>>a;
    cout<<a+1<<endl;
}
```
結果：
16.575 s

快了幾秒，不算太意外，那去掉endl呢？

``` c++
ios_base::sync_with_stdio(false);
for(int i = 0; i < (int)1e7; i++){
    cin>>a;
    cout<<a+1<<'\n';
}
```

結果：
16.408 s

什麼！！完全沒有變快啊！？（你看看，看來就算cout很快，cin還是很慢啊）
等等，已經說過cin沒道理比scanf慢這麼多，所以我們來看看發生了什麼事。
既然和有endl一樣快，我們可以合理懷疑是cin/cout又清空緩衝區了。
我們試試看下面的例子，我們先吃進一個陣列，再丟出來。

``` c++
ios_base::sync_with_stdio(false);
for(int i = 0; i < (int)1e7; i++){
    cin>>A[i];
    cout<<A[i]+1<<'\n';
}
```

結果：
2.918 s
2.811 s
3.062 s

太神奇了，竟然變得甚至比scanf/printf還要快了，發生了什麼事？
看起來是cin/cout交錯使用導致的，我們看一下C++ Reference對於cin的說明，我們發現一個函數tie()。
> std::ios::tie
> Get/set tied stream
> The tied stream is an output stream object which is flushed before each i/o operation in this stream object.

這樣就清楚了，cin預設綁住了cout，而被綁住的ostream會在istream要輸入時被flush。
那我們試試看把cin/cout解綁，我們可以透過傳一個NULL（也可以用0）進入cin.tie()來讓cin綁住空的ostream。
我們加上一行cin.tie(0)再來看剛剛的例子。

``` c++
ios_base::sync_with_stdio(false);
cin.tie(0);
for(int i = 0; i < (int)1e7; i++){
    cin>>a;
    cout<<a+1<<'\n';
}
```

結果：
2.956 s
2.889 s
3.509 s

時間已經和吃進陣列差不多了，剩下的差距已經在誤差範圍內了。

為什麼要有tie這個設計呢？
我曾經看過一些說法，一種是說，因為我們有時候可能要寫一些console應用程式，如果我們要使用者輸入一些值的時候可能要先輸出一些提示訊息像是「請輸入一個數字：」然後才用cin輸入，要是上面那一句話沒有被flush到螢幕上的話，使用者就看不到了，而且你可能不想要換行，就算加<<flush也很麻煩，所以C++就設計了這樣的作法，讓你在cin前會把cout清空緩衝區。

無論如何，如果我們要對檔案輸入輸出很顯然不需要這樣，所以就解綁吧！

## 總結

我們試著把數字範圍放大到1e8看看，

scanf/printf：
30.722 s
29.428 s

cin/cout：
27.052 s
27.097 s

cin/cout的表現已經比scanf/printf好了，事實上，我之前看過一篇文章（現在找不到了QQ）裡面有一張圖表，上面顯示了cin/cout的效率在1e7之後就會開始超越scanf/printf了，當然這有很多的因素在裡面，而且iostream使用的記憶體也比scanf/printf高出一些。

但總結來說cin/cout和scanf/printf比起來更快最主要的原因，是cin/cout可以在編譯時期就把型別等等編譯進去，而scanf/printf則要在執行時期處理，所以cin/cout就算比scanf/printf快，我覺得也不會很奇怪。
