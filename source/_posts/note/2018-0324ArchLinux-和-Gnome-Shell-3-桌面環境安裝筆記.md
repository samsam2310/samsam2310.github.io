---
title: ArchLinux 和 Gnome Shell 3 桌面環境安裝筆記
category:
  - Note
date: 2018-03-24 03:24:19
tags:
 - Linux
 - Archlinux
 - Gnome-Shell
 - Plymouth
---

自從上次灌好 Archlinux 後其實我一直想要來寫一篇筆記，可是一直都沒有真的來寫。
結果過了一年，因為換電腦又重灌了一次，這次來把它寫下來。

這篇的重點會擺在如何安裝一個個人工作用的 Archlinux 環境，其實步驟意外的不難，當然是過程中不要出意外啦XD

Archlinux 好處是單純，乾淨，很容易了解並自己調整內容，也很適合拿來學習熟悉 Linux 系統。加上社群活躍，更新快速，拿來當開發環境也很適合，整體來說都是個值得試試看的系統。

灌好桌面大概長這樣：
{% asset_img screen.png 螢幕節圖 %}

<!--more-->

## 事前準備

首先提一下，如果是沒什麼灌過作業系統，或是沒有灌過 Linux 的人如果下定決心想要熟悉 Linux 無訪，但若只是要搞個 Linux 環境寫作業還是幹麻的，推薦還是去灌 Ubuntu 就好。
Archlinux 是給喜歡自己校調系統的人使用的（？

首先，灌系統還是要準備的有，東西是否都備份了，自己的硬體架構和驅動程式是不是可以被 Archlinux 支援等等的。不過通常都不會太慘啦？灌下去就知道了。
筆電通常問題比較多，因為硬體都是怪怪的硬體。聽說 Dell 的筆電的話會好一點，畢竟有原廠支援 Ubuntu 的樣子。其他的就看緣份吧？
如果你是從其他 Linux 重灌的，記得備份 ssh-key 和 bashrc 等等檔案。

## 安裝 Archlinux

反正都看這邊 ： https://wiki.archlinux.org/index.php/installation_guide
這個步驟和大部分安裝過程類似，就先搞個 Archlinux 隨身碟，燒 Archlinux 的 ISO，接著用它開機等等的，接下來的步驟就照 Archlinux 官網的安裝教學。
燒隨身碟主要好像有兩家軟體，有時候用某一家燒會開不了機就換一家的試試看，不同電腦好像也有不同的狀況。
Archlinux 是用指令一步一步完成安裝的，包括分割硬碟，mount，安裝 OS 基本套件等等可能會嚇到很多新手，不過其實也就那樣而已。

關於分割硬碟，基本上因為是自己個人使用，割一個 / 磁區跟 swap 磁區就好。如果是 SSD 的話就不要 Swap 了太傷 SSD 了。

我灌兩次大概遇過幾個問題：

### Sercurity Boot

這東西記得關掉，他是用來驗證你的 OS 是不是某些受信任的廠商(只有 Windows ??) 發行的，Linux 嘛，反正也不可能有人幫它簽，自己簽也沒什麼意義，所以直接關掉就好。

### Grub2 Efi

如果從舊的 BIOS MBR 開機方式轉過來的人可能不了解 UEFI 開機模式。其實很簡單，灌電腦的時候多割一個 EFI 磁區，然後照著 [Archlinux Grub](https://wiki.archlinux.org/index.php/GRUB) 文件上關於 UEFI 的步驟安裝 Grub 就可以了。
基本概念就是，現在 UEFI 系統會去硬碟裡搜尋 EFI 磁區，從裡面找開機檔來開機，很方便，可以跟 Windows 的 EFI 磁區分開達成雙重開機又不怕被 Windows 蓋回去了。
如果你是用蓋掉 Windows 的 EFI 磁區的方式安裝的要小心 Windows 把他的 EFI 磁區自動修復又搞回去。
如果不小心搞壞了 Windows 的 EFI 去網路上找 Windows 的指令修復就可以了。

### Dell eps/EFI/boot/bootx64.efi

這個最雷了...。想說怎麼 Grub 灌好了卻開不了，結果 Dell 會去檢查你的 eps/EFI/boot/bootx64.efi 這個檔案在不在...。基本上跟你的grub 的 .efi 檔同一層目錄下，新增一個 bootx64.efi 的檔案，就可以動了...。


其他相關問題就去逛逛 Archlinux wiki 吧？


## 套件安裝

這裡寫幾個我安裝的套件：

* openssh vim sudo
這三個就不解釋了，反正會用到，vim 可能安裝系統的時候就會裝了，因為會去條設定檔XDsudo 的話就是先加個使用者當作平常自己用的使用者然後去設定下。
* base-devel git yaourt (更新：yaourt 已經被棄用了，請改成 yay 之類的套件)
base-devel 包含所有用來 build 套件的相關工具，也有 gcc g++ 等等，git 就 git。前面兩個是 yaourt 不可或缺的套件。
yaourt 是一個跟包裝 pacman 的套件管理程式，讓你可以像裝官方套件庫一樣安裝 AUR(Arch User Repository) 上的套件，讓你可以輕鬆安裝各種有的沒的，不然你本來要裝可能要自己 git clone 然後自己編譯再安裝，yaourt 就是幫你做這件事情。裝好之後，以後的 pacman 都用 yaourt 指令取代就可以了。不過 yaourt 本身也在 AUR 上，有兩種裝法，一種是暫時加入法國(?)的某個 arch 套件庫到 pacman，或是體驗一次原汁原味的 AUR 套件安裝，詳細網路上資料很多。

**更新** ：yaourt 已經被棄用了，請改用其他的 aur helper ，我個人現在是用 yay 取代 yaourt 。參考：https://wiki.archlinux.org/index.php/AUR_helpers

* gnome-shell gdm(gdm-plymouth)
如果你想要有桌面系統（個人電腦用純文字界面也是滿難用的），那 gnome-shell 是個好選擇，漂亮美觀，整體 UI 設計我覺的很好，有很多漂亮主題可以套用。如果你想用別的，也沒差？後面解釋細節。
* chinese-font chinese-input-method(ibus-chewing)(ibus-athiy)(run ibus-setup)
記得要裝個中文字體，不然會顯示不了中文！！其他語言也是要記得裝字體。字體的話就自己選一個喜歡的就好。
至於中文輸入法就是個大坑，gnome-shell 預設跟 ibus 結合，我灌不起來其他的又覺得ibus 還可以用，就用了XD
* sakura(Terminal)
因為新版的 gnome-terminal 移除了背景半透明的功能，為了有跟以前一樣的使用體驗，就發現了這個還不錯的 terminal，雖然預設很醜，不過套用設定檔之後就變得很漂亮了。可以參考我的[設定檔](https://github.com/samsam2310/weirc/blob/master/conf/sakura.conf)，記得放到 ~/.config/sakura/ 下面。
* google-chrome sublime-text-dev
這兩個沒意外要用 ARU 裝，有 yaourt 就直接 yaourt -S 就好了。就 Chrome 跟 Sublime Text 3 ，如果要裝 FF 什麼的請自便 （？
* zram(systemd-swap.service)
如果你的電腦只有 SSD ，那可能不會割 swap ，這時候可以使用 zram ，透過壓縮記憶體製造 swap 的效果，基本上可以達到 ram + swap == 3 * ram 大小的效果 ，還滿厲害的。詳細設定可以看[這裡](https://wiki.archlinux.org/index.php/Improving_performance#Zram_or_zswap)，不過其實灌 systemd-swap 這個套件後去設定檔打開然後 systemctl enable 這個服務就好。


## Gnome-Shell 設定

### 基本安裝

安裝方法很簡單，直接用 pacman 或 yaourt 裝就好了，裝好 gnome-shell 後要再裝一個桌面管理程式（可以想成登入畫面），我是裝了 gdm，它會負責登入和幫你啟動 gnome-shell。
基本上裝好後用 systemctl enable gdm 就可以了，沒有什麼特別會遇到的問題。

### 主題相關

接下來就是安裝 gnome-tweak-tool 這個方便的工具，可以用來調整大部分的 gnome-shell 主題相關的設定。接下來就是去網路上找喜歡的主題套用，我使用了 gnome-arc-dark 和 paper-icons-theme-git 這兩個主題，其中後者是 icon-set ，提供好看的應用程式 logo，蠻意外的是它連 chrome sublime 等等的 icon 都有包含。裝好後，去 gnome-tweak-tool 選擇套用。

另外可以裝一些 gnome-shell 的插件，要裝插件用 gnome-shell 預設付的網頁瀏覽器去瀏覽插件網站的話可以用滑鼠點一下 ON 就裝好很方便，Chrome 還是其他瀏覽器理論上也要可以，不過還要條設定什麼的，我是覺得直接用內建的瀏覽器充當插件管理員也不錯xD。
我裝了 dash-to-dock 和 BackSlide 兩個插件。前者是讓你可以把工具列做些調整，比如像 mac 一樣換到下面和一些很好用的功能。後者是可以自動定時換桌布。

背景圖片上至桌布下至 Grub 選單都可以簡單換成自己喜歡的圖片，就只有 GDM 的密碼輸入畫面比較麻煩一點，可以參考 [Arch Wiki](https://wiki.archlinux.org/index.php/GDM#Log-in_screen_background_image)的說明去修改，需要改設定檔並重新產生一些 binary 檔案。

Sakura terminal
{% asset_img screen2.png 螢幕節圖 %}

GDM 輸入密碼畫面
{% asset_img screen3.png 螢幕節圖 %}

螢幕鎖定畫面
{% asset_img screen4.png 螢幕節圖 %}

### 中文輸入法

中文輸入法一直是個痛苦的東西。gnone-shell 3 開始決定整合 ibus 變成一體的輸入法，所以也不建議使用其他輸入法了，因為整合後滿漂亮的，也比較好灌。
除了 ibus 本體外，我選擇了 ibus-chewing 和 ibus-athiy 分別用來輸入中文和日文，如果有其他喜歡的也可以選。選好後，不要照一些以前的怪方法啟用它們，而是去"設定" 中的 "地區和語言" 調整，建議灌完重開機一次最保險，反正我從來沒搞懂怎麼好好裝輸入法...。

接下來記得裝中文字體，要注意的是裝完記得要清除字體 cache 不然會看不到改變（然後你就會覺的怎麼跟原本的一樣醜XDD）。通常都是選 CJK 一整包的字體，日文韓文都會一次包含到，其他字體就照需要安裝就好。

### Plymouth

如果你想要像 Ubuntu 一樣有漂亮的開機動畫，可以灌 Plymouth 這個套件，可以參考 [Arch Wiki](https://wiki.archlinux.org/index.php/plymouth)。
基本上就是灌好，改一改 /etc/mkinitcpio.conf 然後重新 mkinitcpio -p linux。
另外，為了讓開機動畫可以順順的接上 GDM ，可以把 gdm 換成 gdm-plymouth ，先刪掉 gdm 再重裝 gdm-plymouth 並且記得 enable。

接著可以用 plymouth-set-default-theme -l 看有哪些主題可以換，並且用其他參數換成那個主題。主題可以去網路上找，也可以自己改，還滿簡單的。
可以用以下指令測試，tty 可以自己改成空的 tty。


``` bash
plymouthd --tty=tty5 --debug --mode=boot
plymouth --show-splash
sleep 5
plymouth --quit
```

通常這些步驟可能會遇到一些問題，基本上就慢慢摸索就好了。


## 收尾

最後就是安裝一些自己會用到的東西，tweak-tool 可以調的設定滿多的可以摸索一下，比如可以把快速鍵 ctrl+shift+t 設定成 sakura terminal 等等。
目前用起來整體覺得很滿意，界面很漂亮，可以按照自己的意思調整，加上 gnome-shell 對整體的使用者體驗設計上還算不錯。
缺點大概就是目前 gnome-shel l 記憶體吃很兇，幾乎可以確定是 Bug 了但是官方卻遲遲無法解決，只能期待哪天被修復了。

大概就這樣，希望如果有人跟我一樣不想使用跟 WIndows 差不多也只是下一步下一步安裝沒什麼自由空間的 Ubuntu 的話，可以試試看 Archlinux 喔！
