---
title: Lenovo-S540 RTL8723BE Ubuntu Wifi and Bluetooth Driver 問題與解決辦法
category:
  - Node
date: 2016-02-24 12:59:55
tags:
- ubuntu
- driver
---

困擾我快兩年的問題，今天終於解決了。
讓S540上的Ubuntu的Wifi和藍牙完美運作的方法。


## 問題

首先，
問題是這樣的，

我的筆電，S540用的無線網卡是rtl8723be，這是一款藍牙和Wifi二合一的網卡。
在Windows上有良好的支援，可是在Ubuntu 14.04 LST上卻會發生一些狀況。
Wifi的狀況是，一開始看起來很正常，但是過了一段時間後，就會自己斷線，並且無法連線。
重新開機可以暫時修復這個問題，但是過一段時間問題又會出現。

而藍牙則是根本就不能運作，雖然有抓到驅動也有介面，可是卻無法連線，其他裝置也沒辦法掃描到你。

今天我終於找到了解決辦法。

<!--more-->

## 解法1 修改設定（修復WIFI）

於是就這樣過了兩年的用USB連手機網路的生活後，今天心血來潮決定來找找看別人寫的驅動，運氣很好一找就被我找到了。
首先是這篇，http://www.dedoimedo.com/computers/ubuntu-trusty-realtek.html
這篇裡給了一個簡單的解法，也就是在 **/etc/modprobe.d/rtl8723be.conf** 這個檔案裡（如果沒有就新增一個），加入一行設定 
```
options rtl8723be fwlps=N ips=N
```
加入這行的原因是為了避免網卡進入休眠或是省電模式的樣子，總之加了之後重開機，觀察了一段時間，狀況良好。（因為我後來又更新了驅動，所以不確定是不是真的解決了問題，但是我估計應該是OK的）


## 解法2 （修復藍牙）

雖然WIFI修好了，但是藍牙依舊不給力，
查了這篇資料https://github.com/lwfinger/rtlwifi_new/issues/18後，在Github找到了一個驅動程式。
https://github.com/lwfinger/rtl8723au_bt
這個是藍牙驅動，版本是8723的其他幾個版本通用的樣子，其實上面那個rtlwifi_new就是幾個最新的Realtek網卡驅動程式，只是不包含藍牙驅動，作者指示說可以用rtl8723au_bt這個驅動程式。

於是，先把驅動載下來，沒有git的人請先裝git。
```
git clone https://github.com/lwfinger/rtl8723au_bt.git
cd rel8723au_bt
# change branch to new, troy or kernel.
git checkout troy
make
sudo make install
```
他有幾個不同的branch，我試了幾個後發現我灌的起來的只有troy跟new兩個branch，後來確定可以跑的是troy這個，雖然我也不確定他們的確切差別，還有一個branch叫作kernel，是給kernel 3.20以上的人使用的（說法有很多，有地方說3.1４以上也可以，可是我是3.16卻灌不起來），作者說kernel的code是最好的，都可以灌灌看。
灌好了之後，重開機。

重開機之後，我的藍牙就可以用了，但是又出現了另外一個問題，就是只要WIFI開著藍牙就無法使用。（請先把WIFI關起來，再看看藍牙可不可以連線），藍牙連線後只要開WIFI藍牙就會斷線（連線還是連著，但是只要試著傳個檔案之類的就會知道已經斷線了）。


## 解法3 （修復解法2的問題）

照著解法二的步驟做完後，我們可以分別使用藍牙和WIFI了，但是卻不能同時用。
查了資料，資料說可以試試看更新WIFI驅動，不要用內建的。
也就是去安裝 https://github.com/lwfinger/rtlwifi_new 這個驅動。

這個驅動一樣有幾個branch，我選擇灌troy那個。
```
git clone https://github.com/lwfinger/rtlwifi_new
cd rtlwifi_new
git checkout troy
make
sudo make install
```
灌完之後重開機。

之後，我的WIFI跟藍牙就完美運作了！！！！
真是太感動了，世界上就是有這些人幫Linux寫驅動讓世界更美好。


## 總結

雖然可以運作了，不過我記得資料上說，如果Linux更新kernel的話，驅動必須重新安裝。
網路上有找到一個不用重裝的版本，可是我灌不起來，所以就沒用了。

因為裝起來也沒有很久，所以就重新裝吧！
比起每次開電腦都要用USB接手機上網，還要不停擔心USB線接觸不良（最慘的是會不停重新連線導致ubuntu桌面當機QQ），可以連WIFI真的舒服多了，而且藍牙也修好了，除了WIFI也可以用藍牙連手機網路。

爽。
