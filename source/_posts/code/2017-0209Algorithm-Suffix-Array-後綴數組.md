---
title: 'Algorithm::Suffix Array 後綴數組'
tags:
  - C++
  - IOICamp
  - Algorithm
category:
  - Code 備忘錄
  - String
date: 2017-02-09 16:18:44
---


這次去了 IOICamp，從第一次看到到現在過了這麼久，終於學會 Suffix Array 了QAQ。
大概把理解的過程寫下來。

<!--more-->

在處理字串問題的時候，我們常常需要處理一些跟子字串啦、匹配字串等等的問題，當我們需要拿一些東西對一個大字串(約10^5)做一些事情的時候，Suffix Array也就是後綴數組也許就可以幫我們做到。

所謂的後綴數組就是，我們把一個字串的所有的後綴拿去排序，然後紀錄一些數值。比如字串"ababa"變成後綴數組就是：

| Rank | SA | Hei | Suffix
|---|---|---|--------------------
| 0 | 0 | 0 | ababa
| 1 | 2 | 3 | aba
| 2 | 4 | 1 | a
| 3 | 1 | 0 | baba
| 4 | 3 | 2 | ba

實際上我們只會做出前面的 Suffix Array(以下簡稱 SA(x) )，而不會真的把子字串做出來拿去排序。
做成這樣的好處就是我們可以利用它進行二分搜等等的操作，因為每個後綴的前綴就是一個子字串，我們就像是可以對所有的子字串進行二分搜，當然還要搭配一些東西來輔助，像是 Hei(x)，代表每個子字串和它排名前一名的人，前面有多少字元是相等的，搭配這個資訊就可以作到一些事情，當然也可以真對不同題目去找不同的值。

要如何找到一個 SA(x) 呢？
我們要利用一種特別的找法，也就是倍增法。
當然可以直接用 std::sort 來排序，但是字串的比較是 O(n) 有點糟糕，但我們發現因為其實後綴原本是同一個字串，比較的時候我們可以省下一些時間，利用其他人比較後的結果來排序。

首先我們先定義兩個必要的東西， 一個就是 y = SA(x) 代表第 x 名的後綴實際上是從 y 開始的後綴; 另外一個就是 x = RA(y) 代表 y 開始的後綴是第 x 名。
很容易可以發現他們是反函數關係(x = RA(S(x)) )。
然後我們利用 radix sort 的概念，也就是排序字串的時候先比較第一個字元，照著排好序之後，分成很多組，針對第一個字元一樣的人，再對第二個字元排序...。
這時我們可以發現一個性質就是，一個 i 到 i+2*K 的子字串和 j 到 j+2*K 的子字串，想知道它的大小關係，可以透過比較到 i+K 和 j+K 的子字串，如果一樣，再去比較 i+K 到 i+2*K 和 j+K 到 j+2*K 的子字串，反正就是拆成兩塊啦！
我們又知道我們後面的字串已經排好序了，我們就可以利用他們一層一層推出來！

看看這段程式碼：

``` c++
void build_suffix_array(int*SA, int*RA, char*S, bool is_dec=true){
    static int RA2[N];
    int n = strlen(S);
    copy_n(S,n,RA);
    int n_of_diff_rank = 256; // char has 256 different value.
    for(int k=1; k<n; k*=2){
        // Comput SA_{2k} with RA_{k}
        // sort by rank-k ( ignore the string which len < 1+k )
        if(is_dec){ // "aa" < "a"
            iota(SA, SA+n, 0); // init SA to [0,1,2...]
            radix_sort(SA, SA+n-k, RA+k, n_of_diff_rank);
        }else{ // "a" < "aa"
            iota(SA,SA+k,n-k);
            iota(SA+k,SA+n,0); // init SA to [k,k+1...n-1,0,1,...]
            radix_sort(SA+k, SA+n, RA+k, n_of_diff_rank);
        }
        // sort by rank-0 ( include the string which len < 1+k )
        radix_sort(SA, SA+n, RA, n_of_diff_rank);

        // Comput RA_{2k} with SA_{2k}
        RA2[SA[0]] = 0; n_of_diff_rank = 1;
        for(int i=1; i<n; i++){
            if(noteq(SA[i-1], SA[i], k, n, RA) )
                n_of_diff_rank++;
            RA2[SA[i]] = n_of_diff_rank-1;
        }
        copy_n(RA2, n, RA);
        if(n_of_diff_rank == n)break; // all rank are diff => SA is fully sorted
    }
}
```

先把字串的值直接複製到 RA 裡，當作一開始的大小關係的依據，RA 裡面就是紀錄 i 這個位置的後綴是第幾名，一開始有很多重複的值(重複的字母)，經過多次的排序，我們的目標就是讓它變成 0 到 N-1 全部都不一樣的排名！利用這些排名，就可以把 SA(x) 給排序好！！

首先先照字串的第二段的值去排序 SA(x) ，如果有長度不夠的字串(也就是位置再 n-k 之後的後綴)，就當作它沒有第二段，我們可以先放到旁邊不理它。要照第二段排序只要排序 i 的時候參考 RA(i+K) 就可以照第二段排序了！
因為我這裡的範例用的sort是穩定排序，可以透過把長度不夠的字串放到 SA 前面或是 SA 後面決定不夠長的位置要再 a 前面還是在 z 後面(最上面表格裡的例子就是放在後面的，ababa 然後 aba)，不過基本上沒差啦，除非題目有特殊性質要用。

再來第二步就是再照字串第一段的值去排序，要作到這樣就是比較 i 的時候用 RA(i) 去比較就可以了。

到這裡我們已經把依照 K 做出的 SA-2K 做出來了(依照前 2*K 個字元排序的 SA ，依此類推也有 RA-2K)，再來就是用它來更新 RA 讓它也變成 RA-2K ，作法就是先把「名次」設成 1 ，然後從第0名開始看，如果這一個名次跟上一個名次是一樣的( 用上面那種切兩段比較法來比較 )，那這個人就跟它前一個人並列第 x 名，做完之後我們就得到依照 2*K 的 RA-2K 了。

最後，如果名次已經跟 n 一樣多了那代表已經可以排序所有後綴了，不然就把 K 變成兩倍，如果長度超過 n 那代表也不用繼續了，如果都不是那就從第一步重來。


另外比較有趣的是這段 radix sort：

``` c++
void radix_sort(int*bg, int*ed, int*rank, int n_of_diff_rank){
    static int box[N], tmp[N];
    int n = ed-bg;
    fill_n(box, n_of_diff_rank, 0);
    for(int i=0; i<n; i++)
        box[rank[i] ]++;
    partial_sum(box, box+n_of_diff_rank, box);
    for(int i=n-1; i>=0; i--)
        tmp[--box[rank[bg[i]] ]] = bg[i];
    copy_n(tmp,n,bg);
}
```
先把會有的不同的值的範圍先初始化好，然後用 partial_sum 轉成前綴和！！！最後用倒序的順序把東西填回去，這樣就可以作到穩定的排序，蒸的很蚌！


最後就是怎麼做出 hei 數組，單純的做的話就是跟前一個人比較，算出有幾個一樣，當然這樣很慢。
有一個性質是，AAB 和它前面一個位置 BAAB ，你可以看到它只往前多一個 B 而已，所以你前面一名和它前面一名你可以大膽的猜，要不是他的 hei 往前多 1 ， 要不然就是它跟它比的人前面多的不一樣變成 0 ，反正它最多 1 所以你的 hei 就是大於等於他的 hei -1 ！！！
所以你只要照著位置順序做，每次做下個位置的時候只要從上一個 hei -1 開始找就好，這樣複雜度就是字串最長 n + 每次下一個位置長度最多減一就是好好的 O(2*n)。

``` c++
void build_hei(int*SA, int*RA, char*S, int*Hei){
    int k = 0, n = strlen(S);
    for(int i=0;i<n;i++){
        if(RA[i]){
            while(S[i+k]==S[SA[RA[i]-1]+k])k++;
        }
        Hei[RA[i]] = k;
        k = max(0,k-1); // hei(rank(i+1) ) >= hei(rand(i) )-1
    }
}
```

最後說個簡單的例題，比如要你求一個字串的不相等的子字串有幾個？
作法就是把 SA 求出來之後做 Hei ，然後所有子字串的可能個數扣掉 Hei 的總和(剛好就是重複的子字串數)。

下面就是完整的程式碼。
實做上可以再壓一點變數之類的，不過為了寫清楚就這樣寫了。


``` c++
#include <bits/stdc++.h>
using namespace std;
const int N = 1e5+1;
void debug_suffix(int *SA, int *RA, int *Hei, char *S, const char*s="suffix array:"){
    int n = strlen(S);
    cout<<s<<endl;
    cout<<"SA Hei:"<<endl;
    for(int i=0;i<n;i++)cout<<SA[i]<<' '<<Hei[i]<<' '<<setw(n)<<(S+SA[i])<<endl;
    cout<<"RA:"<<endl;
    for(int i=0;i<n;i++)cout<<RA[i]<<' '<<setw(n)<<(S+i)<<endl;
    cout<<"-------"<<endl;
}
bool noteq(int a, int b, int k, int n, int*RA){
    return RA[a] != RA[b] || a+k>=n || b+k>=n || RA[a+k] != RA[b+k];
}
void radix_sort(int*bg, int*ed, int*rank, int n_of_diff_rank){
    static int box[N], tmp[N];
    int n = ed-bg;
    fill_n(box, n_of_diff_rank, 0);
    for(int i=0; i<n; i++)
        box[rank[i] ]++;
    partial_sum(box, box+n_of_diff_rank, box);
    for(int i=n-1; i>=0; i--)
        tmp[--box[rank[bg[i]] ]] = bg[i];
    copy_n(tmp,n,bg);
}
void build_suffix_array(int*SA, int*RA, char*S, bool is_dec=true){
    static int RA2[N];
    int n = strlen(S);
    copy_n(S,n,RA);
    int n_of_diff_rank = 256; // char has 256 different value.
    for(int k=1; k<n; k*=2){
        // Comput SA_{2k} with RA_{k}
        // sort by rank-k ( ignore the string which len < 1+k )
        if(is_dec){ // "aa" < "a"
            iota(SA, SA+n, 0); // init SA to [0,1,2...]
            radix_sort(SA, SA+n-k, RA+k, n_of_diff_rank);
        }else{ // "a" < "aa"
            iota(SA,SA+k,n-k);
            iota(SA+k,SA+n,0); // init SA to [k,k+1...n-1,0,1,...]
            radix_sort(SA+k, SA+n, RA+k, n_of_diff_rank);
        }
        // sort by rank-0 ( include the string which len < 1+k )
        radix_sort(SA, SA+n, RA, n_of_diff_rank);

        // Comput RA_{2k} with SA_{2k}
        RA2[SA[0]] = 0; n_of_diff_rank = 1;
        for(int i=1; i<n; i++){
            if(noteq(SA[i-1], SA[i], k, n, RA) )
                n_of_diff_rank++;
            RA2[SA[i]] = n_of_diff_rank-1;
        }
        copy_n(RA2, n, RA);
        if(n_of_diff_rank == n)break; // all rank are diff => SA is fully sorted
    }
}
void build_hei(int*SA, int*RA, char*S, int*Hei){
    int k = 0, n = strlen(S);
    for(int i=0;i<n;i++){
        if(RA[i]){
            while(S[i+k]==S[SA[RA[i]-1]+k])k++;
        }
        Hei[RA[i]] = k;
        k = max(0,k-1); // hei(rank(i+1) ) >= hei(rand(i) )-1
    }
}
char S[N];
int SA[N], RA[N], Hei[N];
int main(){
    int t;
    cin>>t;
    while(t--){
        cin>>S;
        build_suffix_array(SA,RA,S,true);
        build_hei(SA,RA,S,Hei);
        debug_suffix(SA,RA,Hei,S,"Suffix Array(DES)");
        build_suffix_array(SA,RA,S,false);
        build_hei(SA,RA,S,Hei);
        debug_suffix(SA,RA,Hei,S,"Suffix Array(AES)");
    }
}
```