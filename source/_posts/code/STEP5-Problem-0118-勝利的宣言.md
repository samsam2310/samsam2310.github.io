---
title: 'STEP5::Problem 0118 : 勝利的宣言'
tags:
  - C++
  - Step5
  - Disjoin Set
category:
  - Code 備忘錄
  - Data Structure
date: 2014-03-06 04:42:00
---


http://web2.ck.tp.edu.tw/~step5/probdisp.php?pid=0118
這題是互斥集的問題，用併查樹實作。
問題是有一段未知數列，由0和1構成，題目會有Q筆條件，每筆條件會給你一個區間和該區間有奇數或偶數個1，問你第幾筆條件開始出現矛盾。

<!--more-->

實作方法是用併查樹(集)，一種樹狀結構。一開始每個點的父節點都是自己，然後將同一個集合的元素連起來，所有元素都指向同一個樹根，樹根就只像自己，可以快速判斷兩個元素是否在同一個集合中。
先建一個2*N大小的陣列，元素I代表1到I之間的奇偶性質，類似前綴和的概念。如果題目跟你說A和B的奇偶性質是偶數 ，代表 B前綴和減A-1前綴和是偶數，只有奇數減奇數或偶數減偶數才會是偶數，所以A-1和B要連在一起，A-1'和B'連在一起；反之，相減是奇數了話，就將A-1連到 B'，B連到(A-1)'，至於為何會有B'，這是代表B的相反性質，可以用"B+題目給的區間最大值" 表示，這也是為什麼陣列要開2*N。
因為這樣，所以當B和B'被連到同一個集合中時，我們就知道，發生矛盾了。(因為B和B的相反性質不可能一樣)。
我的作法就是每次將該連的連起來，並檢查該筆條件的B和B'是否在同一個集合理，別懷疑，只要其他數字有問題，B就會有問題，所以不用一一檢查。

關於併查樹http://acoj.twbbs.org/blog_article.php?id=4



``` c++
#include <cstdlib>
#include <iostream>
#include <map>
#define N 5001
using namespace std;
map<int,int> mp;
int find(int a)
{
    if(mp[a]==0){
            mp[a]=a;
            return a;
    }
    while(mp[mp[a]]!=mp[a])mp[a]=mp[mp[a]];
    return mp[a];
}
int link(int a,int b)
{
    mp[find(b)]=find(a);
}
int main(int argc,char *argv[])
{
    ios_base::sync_with_stdio(false);
    int l,n,a,b;
    char S[10];
    cin>>l>>n;
    l++;
    for(int i=0;i<n;i++){
            cin>>a>>b>>S;
            b++;
            if(S[0]=='e'){
                    link(a,b);
                    link(a+l,b+l);
            }else{
                    link(a,b+l);
                    link(b,a+l);
            }
            if(find(b)==find(b+l)){
                    cout<<i<<endl;
                    goto end;
            }
    }
    cout<<n<<endl;
    end:;
    //system("pause");
    return 0;
}
```