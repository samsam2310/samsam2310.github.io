---
title: 'TIOJ::1241 . 倍因道 Factor and Points'
tags:
  - C++
  - TIOJ
category:
  - Code 備忘錄
  - Math
date: 2014-12-15 08:51:41
---


http://tioj.infor.org/problems/1241

這題有點利用質數篩法的概念。

<!--more-->

題目說，給一個N，問1到N之間的數，你要把他分成兩堆，一堆是因數，一堆是倍數，其中只要因數那堆裡某一個數可以整除倍數那堆中的一個數字就得一分，最多總共得幾分。

首先開一個陣列，記錄這個數字有幾個因數(題目定義的因數，不是數學上的因數)，然後開始跑迴圈，對於每個數字，**如果因數的個數小於倍數個數**(N除以這個數，也就是這個數小於N的倍數有幾個，也就是這個數當因數可以得幾分)，則用類似篩法的方式讓後面的數字增加因數個數，不然就把這個數的因數個數加進答案裡。



``` c++
#include <cstdio>
#include <cstring>
#define F(n) Fi(i,n)
#define Fi(i,n) Fl(i,0,n)
#define Fl(i,l,n) for(int i=l;i<n;i++)
#define N 2001
int D[N];
int main(){
    int n,ans;
    scanf("%*d");
    while(~scanf("%d",&n)){
        ans=0;
        memset(D,0,sizeof(D));
        Fl(i,1,n+1){
            if(n/i-1>D[i]){
                for(int j=i+i;j<=n;j+=i)D[j]++;
            }else ans+=D[i];
        }
        printf("%d\n",ans);
    }
}
```