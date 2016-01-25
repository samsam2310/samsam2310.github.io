---
title: 'STEP5::Problem 0110 : 你的法律事務所'
tags:
  - C++
  - Step5
category:
  - Code 備忘錄
  - Else
date: 2014-02-19 05:12:42
---


http://web2.ck.tp.edu.tw/~step5/probdisp.php?pid=0110
題目要求交換陣列上的數字或輸出。
兩個交換就直接換，整行或整列交換要用一組索引，交換索引而不是交換陣列本身。

<!--more-->

其實我寫完這題後發現一個重要的技巧，這種輸入字元A做a輸入B做b的題目，常會因為scanf("")會讀到\n或是一堆問題發生錯誤，其實只要把for迴圈的i++移到if、else if裡面，這樣不論讀到甚麼字元，只要不符合題目都會被忽略，可以減少錯誤的發生。

輸入陣列時可以直接一層for()包住 scanf("%s",array[i]);就行了。

``` c++
#include <cstdio>
#include <cstdlib>
#include <iostream>
#include <cstring>
#define N 3001
using namespace std;

int st[N][N],ax[N],ay[N],pu;

void pp(int T[][N],int n){
       for(int i=0;i<n;i++){
               for(int j=0;j<n;j++)
                       printf("%d ",T[ax[i]][ay[j]]);
               puts("");
               }
       puts("");
}

int main(int argc, char *argv[])
{
  int n,q,t,x,y,x2,y2;
  scanf("%d",&n);
  for(int k=0;k<n;k++){
            ax[k]=k;
            for(int l=0;l<n;l++){
                    ay[l]=l;
                    scanf("%d",&st[k][l]);
            }
  }
  scanf("%d",&q);
  char a2[10];
  for(int k=0;k<q;){
        for(int h=0;h<10;h++)
                a2[h]=0;
        scanf("%s",a2);
        if( strcmp(a2, "Kurae") == 0){
               k++;
               scanf("%d %d",&x,&y);
               x--;
               y--;
               printf("%d\n",st[ax[x]][ay[y]]);
        }else if( strcmp(a2, "Igiari") == 0){
               k++;
               scanf("%d %d %d %d",&x,&y,&x2,&y2);
               x--;y--;x2--;y2--;
               pu=st[ax[x]][ay[y]];
               st[ax[x]][ay[y]]=st[ax[x2]][ay[y2]];
               st[ax[x2]][ay[y2]]=pu;
               //pp(st,n);
        }else if( strcmp(a2, "Matta") == 0){
               k++;
               scanf("%d %d %d",&t,&x,&y);
               x--;y--;
               if(t==0){pu=ax[x];ax[x]=ax[y];ax[y]=pu;}
               else{pu=ay[x];ay[x]=ay[y];ay[y]=pu;}
               //pp(st,n);
        }
}
  //system("pause");
  return 0;
}
```