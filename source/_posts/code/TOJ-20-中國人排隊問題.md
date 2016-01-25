---
title: 'TOJ::20 / 中國人排隊問題'
tags:
  - C++
  - TOJ
  - Link List
category:
  - Code 備忘錄
  - Data Structure
date: 2014-03-25 05:38:21
---


http://2014.sprout.csie.org/oj/pro/20/
題目是有好幾組人，只要同意組人要加入排隊會加到同一組人的最後面，如果沒有同一組人在排隊就直接從最後面排。

<!--more-->

我先用一個陣列紀錄每個人所屬的團體(編號0~1000)，然後一個陣列紀錄那個人後面排的是誰，一個1000的陣列紀錄那團人在隊伍中最後一個是誰，一個fst紀錄第一個是誰，一個lst紀錄最後一個是誰。
之後就開始O(1)修改查詢，加入人就加在那團人最後或是最後面，沒有的話值是-1，離開排隊就直接讓fst指向第一個人指的人。



``` c++
#include <cstdio>
#include <cstdlib>
#include <iostream>
#define N 10000000
using namespace std;
int HK[N]={0},L[N],K[1001];
void PP(int fst){
     printf("GG");
     for(int i=fst;i!=-1;i=L[i])printf(" %d(%d)",i,L[i]);
     puts("");
}
int main(int argc,char *argv[])
{
    int n,k,tmp,t=1;
    while(scanf("%d",&n)!=EOF){
            printf("Line #%d\n",t);t++;
            int fst,lst;
            fst=lst=-1;
            fill(HK,HK+N,0);
            for(int i=1;i<=n;i++){
                    scanf("%d",&k);
                    for(int j=0;j<k;j++){
                            scanf("%d",&tmp);
                            HK[tmp]=i;
                    }
            }
            fill(L,L+N,-1);
            fill(K,K+1001,-1);
            char c[20];
            while(scanf("%s",c)){
                    if(c[0]=='E'){
                          scanf("%d",&tmp);
                          if(fst==-1){
                                  fst=lst=tmp;
                                  if(HK[tmp])K[HK[tmp]]=tmp;
                                  L[tmp]=-1;
                          }else if(K[HK[tmp]]!=-1&&K[HK[tmp]]!=lst){
                                  L[tmp]=L[K[HK[tmp]]];
                                  K[HK[tmp]]=L[K[HK[tmp]]]=tmp;
                          }else{
                                  lst=L[lst]=tmp;
                                  if(HK[tmp])K[HK[tmp]]=tmp;
                                  L[tmp]=-1;
                          }
                    }else if(c[0]=='D'){
                          if(fst==K[HK[fst]])K[HK[fst]]=-1;
                          printf("%d\n",fst);
                          fst=L[fst];
                    }else if(c[0]=='S')break;
                    //PP(fst);
            }
    }
    //system("pause");
    return 0;
}
```