---
title: 'STEP5::Problem 0094 : 致兩千年後的你'
tags:
  - C++
  - Step5
  - Sort
category:
  - Code 備忘錄
  - Else
date: 2014-03-06 04:43:25
---


http://web2.ck.tp.edu.tw/~step5/probdisp.php?pid=0094
這題是簡單的排序加區間和最大值搜尋。

<!--more-->

一切都是從這首歌開始說起...........

{% youtube w5SH23Xy9d8 %}

總之就是給你多筆事件，由時間、警告度、番茄汁量組成。求連續N個時間中警告度和的最大值，如果警告度一樣，第二筆序就是番茄汁(笑... )

我拿來練習重載運算子。因為自己建的struct沒有運算規則(廢話....)，丟到sort()叫他排序他一定擺臭臉給你CE，所以要先定義你的struct的運算規則，幾本上STL的比大小都用小於'<'符號比較，所以定義小於就可以讓STL正常運作。

基本上先打 inline bool  operator< (const TYPE NAME a,const  TYPENAME  b){....你的函式...} inline(可有可無) bool (比大小當然回傳bool) operator< (關鍵字operator 加上符號就可以定義運算規則) const (把變數設成唯讀，不這樣編譯不會過)  TYPE NAME (看你的sturct 名字叫什麼，宣告兩個吧) 這樣寫完後，就可以開心的在程式裡用  TYPE NAME  a,b;  if(a>b)  這種語法了， 其他加減乘除也可以重載，回傳值改成你的結構名字就可以了。

這題我定義完運算子就丟到sort裡照時間從小排到大，然後檢查寬度是N的區間的和，記錄最大值並輸出。



``` c++
#include <cstdlib>
#include <iostream>
#include <algorithm>
#define N 1000001
using namespace std;
struct MP{
    void c(){a=b=0;}
    int t,a,b;
}KK[N];
inline bool operator<(const MP a,const MP b)
{
    return a.t<b.t;
}
inline bool operator>(const MP a,const MP b)
{
    return (a.a==b.a)? ((a.b>b.b)? true:false ): ((a.a>b.a)? true:false);
}
inline MP operator+(const MP a,const MP b)
{
    return (MP){0,a.a+b.a,a.b+b.b};
}
inline MP operator-(const MP a,const MP b)
{
    return (MP){0,a.a-b.a,a.b-b.b};
}
int main(int argc,char *argv[])
{
    ios_base::sync_with_stdio(false);
    int k,n,t,a,b;
    cin>>k>>n;
    for(int i=0;i<k;i++){
            cin>>t>>a>>b;
            KK[i]=(MP){t,a,b};
    }
    sort(KK,KK+k);
    MP tmp;tmp.c();
    for(int i=0;i<n;i++){
            tmp=tmp+KK[i];
    }
    MP max=tmp;
    for(int i=0;i<k-n;i++){
        tmp=tmp-KK[i];
        tmp=tmp+KK[n+i];
        // cout<<tmp.a<<" "<<tmp.b<<" "<<n+i<<endl;
        if(tmp>max)max=tmp;
    }
    cout<<max.b<<endl;
    // system("pause");
    return 0;
}
```