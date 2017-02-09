---
title: Algorithm-DP優化之四邊形不等式優化
tags:
  - C++
  - Algorithm
category:
  - Code 備忘錄
  - DP
date: 2016-04-02 13:41:02
---


最近寫了一些四邊形不等式優化的題目，也好好的重新看了一下之前看過的資料，終於對四邊形不等式有比較深的理解了，決定把它寫下來。

<!--more-->

## 概念

所謂的四邊形不等式優化，是一種DP優化的技巧。
所謂的DP優化，就是在某些特殊的式子滿足某些條件後，我們可以用特定的順序、特定的資料結構來轉移，進而減少複雜度。其中四邊形不等式就是其中一種，另外常見的DP優化還有單調隊列優化、斜率優化等等。

所謂的四邊形不等式，就是如果我們現在有一個函數 $ f(i,j) $，
若 $ a<b\leq c<d, f(a,c) + f(b,d) \geq f(a,d) + f(b,c) $，則 f 滿足凹四邊形單調性。
若 $ a<b\leq c<d, f(a,c) + f(b,d) \leq f(a,d) + f(b,c) $，則 f 滿足凸四邊形單調性。

這兩個不等式在某些DP的情況下已經被證明出可以有某些優化了，所以當問題的DP轉移符合條件，又符合四邊形不等式，我們就可以使用某些優化。

在開始講之前，DP有些專用的術語， eD/tD 指的是有 $ N^e $ 子問題，每個子問題要依賴 $ N^t $ 個子問題來轉移。(由此可知總時間複雜度是 $ O(N^{e+t}) $)


## 1D/1D 凹性優化

通常 1D/1D 的轉移式長得像這樣：
$$ f(i) = min \lbrace f(j) + w(j)\ |\ 0\leq j \leq i-1 \rbrace $$

這個優化有一個例題：[IOI 2015 Teams](/code-2016-0316TIOJ-IOI-1886-%E3%80%90IOI2015%E3%80%91Teams-%E4%B8%80%E5%A0%86%E6%8F%90%E5%A7%86/)。
這題的其中一個解法裡有用到一個DP式：
$$ dp[i] = min \lbrace dp[j] + z(j,i) - k_i | 0 \leq j \leq i-1 \rbrace $$
其中他滿足式1：
$$ dp[i] + z(i,k) \leq dp[j] + z(j,k) \Rightarrow dp[i] + z(i,d) \leq dp[j] + z(j,d); (k<d)$$
證明在連結裡，這個式子其實是從四邊形不等式轉過來的，可以把四個數字帶入四邊形不等式：
$$ f(i,j) = dp[i] + z(i,j) $$
$$ f(i,k) + f(j,d) \geq f(j,k) + f(i,d) ; (i<j<k<d) $$
由上面的式子，你就會發現式1就會對了，因為$ f(i,k) \leq f(j,k) $ ， $ f(j,d) \geq f(i,d) $ 就不得不成立，否則就矛盾了。

而有了這個條件，我們可以發現當一個 i 在某一個時刻比 j 好時( i<j )，他之後就會永遠比 j 好，於是我們用一個 list 存放當前的 id ，並用一個 vector 存放 list 的 iterator ，然後再開一個 heap 維護每個 id 什麼時候把他下一個殺掉(什麼時候會比下一個好)，接著再轉移前先從 heap 裡拿出已經過期的元素，用 vector 裡的指標找到他並把他 erase 掉，再重新計算這個數字的下一個人的死亡時間，最後用 list 找到最後一個元素，因為在更新完後保證了每個元素一定都比下一個差(如果比下一個好就會把下一個殺掉)，所以轉移最好的 id 就是 list 的最後一個元素。
這裡用 list 可以 O(1) 找到最後一個元素，用 vector 是為了 O(1) 找到 list 裡的某個東西。
我稱這個資料結構叫做 Magic。XDD

## 1D/1D 凸性優化

和凹性很類似，可是變成某個 id 過了一個時間後就會永遠比較差，所以把 Magic 結構裡，前面殺後面改成後面殺前面，然後找 list 的最前面的元素來轉移。

以上兩個優化把複雜度從$ O(N^2) $優化成$ O(N\ logN) $

## 2D/1D 凹性優化

通常 2D/1D 的轉移式是長這樣的：
$$ f(i,j) = min \lbrace f(i,k) + f(k,j) + w(i,j)\ |\ i \leq k \leq j \rbrace $$

可以枚舉 i ，之後每一維就用 1D/1D 凹性優化來解，複雜度 $ O(N^2\ logN) $


## 2D/1D 凸性優化

這個優化有一個例題：[郵局設置問題EXTREME](/code-2016-0402TIOJ-1449-%E9%83%B5%E5%B1%80%E8%A8%AD%E7%BD%AE%E5%95%8F%E9%A1%8CEXTREME)。
這題的轉移式和一般的 2D/1D 的轉移式有一些不同，所以讓我想了很久才想通。

這裡證明一下比較通用型的式子，針對不同題目會有一些小差異，最好重新證明一次才不會爛掉。
2D/1D 凸性優化，最後我們會想要得到一個結果 $ K(i,j-1) \leq K(i,j) \leq K(i+1,j) $， K(i,j) 代表的是 [i , j] 中用來轉移的最好的 k 。
有了這個性質，我們用特定順序，這裡就是 i 從 N-1 到 0 ， j 從 0 到 N-1 ，每次轉移就紀錄現在最好的 k 是誰，當我們要轉移 i,j 的時候，我們的迴圈裡的 k 只要從 $ [K(i,j-1), K(i+1,j)] $ 這個區間裡枚舉就好，總複雜度就會變成 $O(N^2)$。

現在來證明這件事情，
式1：$ a<b\leq c<d, f(a,c) + f(b,d) \leq f(a,d) + f(b,c) $ ，也就是四邊形不等式。
式2：$ f(i,k) + f(k,j) \geq f(i,d) + f(d,j); (k<d)$ ，假設 d 是轉移 f(i,j) 最好的斷點(也就是$K(i,j)$)，那所有小於 d 的 k 當然結果都會比用 d 轉移大。

假設現在當 $k<d$ ， $ f(i+1,k) + f(k,j) \geq f(i+1,d) + f(d,j) $ 成立的話， $ d = K(i,j) \leq K(i+1,j) $ ，因為我們知道所有小於 d 的 k 都比 d 還要差， $K(i+1,j)$ 不可能落在 $K(i,j)$ 之前。

現在來證明這個假設，
由式1可以得到 $ f(i+1,k) + f(i,d) \geq f(i+1,d) + f(i,k); (i < i+1 < k < d) $
$ \Rightarrow f(i+1,k) + f(k,j) + f(i,d) + f(d,j) \geq f(i+1,d) + f(d,j) + f(i,k) + (k,j) $ ， 兩邊同加 $f(k,j)+f(d,j)$
$ \Rightarrow (f(i+1,k) + f(k,j) + f(i,d) + f(d,j)) - (f(i+1,d) + f(d,j) + f(i,k) + f(k,j)) \geq 0$ 
$ \Rightarrow (f(i+1,k) + f(k,j)) - (f(i+1, d) + f(d,j)) \geq (f(i,k) + f(k,j)) - (f(i,d) + f(d,j))$ 
因為式2， $ (f(i,k) + f(k,j)) - (f(i,d) + f(d,j)) \geq 0 $ 
$ \Rightarrow (f(i+1,k) + f(k,j)) - (f(i+1, d) + f(d,j)) \geq 0$ ，
$ \Rightarrow f(i+1,k) + f(k,j) \geq f(i+1,d) + f(d,j) $ ， QED。

同理可證 $K(i,j-1) \leq K(i,j)$，所以最後就可以得到 $ K(i,j-1) \leq K(i,j) \leq K(i+1,j) $。


## 參考資料

這篇基本上是抄學長的Blog還有學長的講義，自己再理解一遍而已，大家可以去看看原本的資料。
學長的Blog：http://momo-funnycodes.blogspot.tw/2012/06/tioj-1449-extreme.html
建中資訊培訓講義： http://pisces.ck.tp.edu.tw/~peng/index.php?action=showfile&file=f4c79992d9e0b5ee4aa9fd7db9115c673b2cf6150
