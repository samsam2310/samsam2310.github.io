---
title: 'TOJ::Matrix'
tags:
  - C++
  - TOJ
category:
  - Code 備忘錄
  - Math
date: 2014-02-25 06:00:01
---


題目是要求一段數字S， Bij=Si*Sj，矩陣B的任意子矩陣和有多少等於a。
觀察之後發現，某一個子矩陣的和等於他的邊(數列S)的乘積，有點類似(A+B)的平方之類的。

<!--more-->

所以我每舉N平方的S的區間和，如果是a的因數就把 S[a]++，最後再跑一次 S[i]*S[a/i]，也就是因數分解a的感覺。如果a是0就S[i]*S[0]*2，因為不是0時會跑到兩次，但是0只會跑到一次所以要*2，還要記得加上S[0]*S[0]。
注意範圍，答案最大的狀況是 (4000^2)^2



``` c++
#include <cstdio>
#include <cstdlib>
#include <iostream>
#include <cstring>
#define N 4001
#define LL long long
using namespace std;
LL S[N*9]={0};
int NU[N];
char C[N];
int main(int argc,char *argv[])
{
    int t,a;
    LL sum=0,tmp=0;
    scanf("%d",&t);
    while(t--){
        sum=tmp=0;
        fill(S,S+N*9,0);
        scanf("%d",&a);
        scanf("%s",C);
        int n=strlen(C);
        for(int i=0;i<n;i++){NU[i]=C[i]-'0';}
        for(int i=0;i<n;i++){
                sum=0;
                for(int j=i;j<n;j++){
                        sum+=NU[j];
                        if(a==0)S[sum]++;
                        else if(sum==0){}
                        else if(a%sum==0)S[sum]++;
                }
        }
        tmp=0;
        if(a==0){
            for(int i=1;i<=N*9;i++){
                tmp+=S[0]*S[i]*2;
                //if(S[i]!=0)printf("EE:(%d)%d %d",i,S[0],S[i]);
            }
            tmp+=S[0]*S[0];
            printf("%lld\n",tmp);
        }else{
            for(int i=1;i<=N*9;i++){
                if(S[i]!=0){
                    if(a/i<=N*9)tmp+=S[a/i]*S[i];
                }
            }
            printf("%lld\n",tmp);
        }
        
    }
    return 0;
}
```