---
title: 'STEP5::Problem 0085 : 切木棒'
tags:
  - C++
  - Step5
  - 背包問題
category:
  - Code 備忘錄
  - DP
date: 2014-02-21 05:43:02
---


http://web2.ck.tp.edu.tw/~step5/probdisp.php?pid=0085.
我是用背包問題解這題。

<!--more-->

背包大小是木棒的長度，先照木棒的價錢排序(要記錄長度_i和價錢_i)，我用了一個sturct紀錄，sort()排序(sort()在裡，我常常忘記.....)，因為一個長度可以切兩次，所以從DP[1]跑到DP[L](01背包是從後面跑回來。)
DP[i]=MAX(DP[i],DP[i-長度_i]+價錢_i)


``` c++
#include <cstdio>
#include <cstdlib>
#include <iostream>
#include <algorithm>
#define N 100001
using namespace std;
struct MP{int f,s;}S[N];
inline bool operator<(const MP a,const MP b){
        return (a.s>b.s)? true:(a.s<b.s)? false:(a.f>b.f)? true:false;
}
int DP[N];
int main(int argc,char *argv[])
{
    int l;
    scanf("%d",&l);
    for(int i=1;i<=l;i++){
            scanf("%d",&S[i].s);
            S[i].f=i;
    }
    //puts("AA");
    sort(S+1,S+l+1);
    for(int j=1;j<=l;j++)
        for(int i=1;i<=l;i++)
                if(i-S[j].f>=0)
                    DP[i]=(DP[i]>DP[i-S[j].f]+S[j].s)? DP[i]:DP[i-S[j].f]+S[j].s;
    printf("%d\n",DP[l]);
    //for(int i=0;i<11;i++)printf("%d ",DP[i]);
    //system("pause");
    return 0;
}
```