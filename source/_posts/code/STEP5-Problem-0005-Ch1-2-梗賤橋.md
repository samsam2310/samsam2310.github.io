---
title: 'STEP5::Problem 0005 : Ch1-2.梗賤橋'
date: '2014-02-25 08:08:47'
tags:
  - C++
  - Step5
category:
  - Code 備忘錄
  - Math
---


http://web2.ck.tp.edu.tw/~step5/probdisp.php?pid=0005
這題題目要我們先換位置再判斷第m個木板是不是質數。

<!--more-->

首先先建質數表，move[數字]=位置，用質數表去跑這個(交換位置)，然後再用lu[數字]=位置轉換回位置對到數字。這樣好像是(根號N)^2=O(N)吧...
之後O(1)查詢，如果 lu[m] 是質數(g[]是篩質數的bool陣列)直接輸出質數，另外如果 lu[m]比n大，代表這一個是n以下最後一個合數的位置(質數應該移到這格，只是我跑的是n最大的狀態，所以這格會是大於n的合數)，輸出質數。
其他狀況就直接輸出 lu[m]。


``` c++
#include <cstdio>
#include <cstdlib>
#include <iostream>
#define M 1000001
using namespace std;
int move[M],lu[M],st[350000],sta=0; //move[數字]=位置 lu[位置]=數字
bool g[M]={0};
int main(int argc,char *argv[])
{
    g[1]=1;  //建質數表
    for(int i=2;i<M;i++){
            if(!g[i]){
                      st[sta++]=i;
                      for(int j=2;i*j<M;j++)
                              g[i*j]=1;
            }
    }
    for(int i=1;i<M;i++)  //交換數字的位置
            move[i]=i;
    for(int i=0;st[i]<M/2;i++){
            int T=st[i];
            for(int j=2;T*j<M;j++){
                    int tmp=move[T];
                    move[T]=move[T*j];
                    move[T*j]=tmp;
            }
    }
    for(int i=1;i<M;i++)   //把位置對應到數字
            lu[move[i]]=i;
    for(int i=0;i<100;i++)
            printf(" %d",lu[i]);
    int t,n,m;
    scanf("%d",&t);
    for(int i=0;i<t;i++){
            scanf("%d %d",&n,&m);
            if(lu[m]>n||!g[lu[m]])  
                      puts("Geng Jian malheureux roi mauvaise!!");
            else
                      printf("%d\n",lu[m]);
    }
    //system("pause");
    return 0;
}
```