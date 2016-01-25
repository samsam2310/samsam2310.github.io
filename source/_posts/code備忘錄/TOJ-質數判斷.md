---
title: 'TOJ::質數判斷'
tags:
  - C++
  - TOJ
category:
  - Code 備忘錄
  - Math
date: 2014-02-23 05:47:15
---

突然發現我對基礎的質數建表非常不熟悉，以下兩種方法，因為第二筆詢問數太多範圍又比較小，所以直接建表；第一筆則是建表到根號N，在用除法判斷，在篩質數時，要注意迴圈的範圍，其實這裡沒什麼大問題，就是如何簡化code和速度而已。


<!--more-->



``` c++
//第一筆  建表到根號N，每筆詢問試除
#include <cstdio>
#include <cstdlib>
#include <iostream>
#define N 46342
using namespace std;
bool S[N]={0};
int ST[46342],st=0;
int main(int argc,char *argv[])
{
    S[0]=S[1]=1;
    for(int i=2;i<N;i++){
            if(!S[i]){
                      ST[st++]=i;
                      if(i<216)for(int j=i*i;j<=N;j+=i)S[j]=1;
            }
    }
    int t,a;
    scanf("%d",&t);
    while(t--){
               scanf("%d",&a);
               if(a<=N){
                       if(S[a])puts("no");
                       else puts("yes");
               }
               else{
                       for(int i=0;i<ST[i]*ST[i]<=a&&i<st;i++){
                               if(a%ST[i]==0)goto no;
                       }
                       puts("yes");
                       continue;
                       no:;
                       puts("no");
               }
    }
    return 0;
}
//第二筆，直接建表到N，O(1)查詢
#include <cstdio>
#include <cstdlib>
#include <iostream>
#define N 10000001 
using namespace std;
bool S[N]={0};
int main(int argc,char *argv[])
{
    S[0]=S[1]=1;
    for(int i=2;i<N;i++){
            if(!S[i]){
                      if(i<4000)for(int j=i*i;j<N;j+=i)S[j]=1;
            }
    }
    int t,a;
    scanf("%d",&t);
    while(t--){
               scanf("%d",&a);
               if(S[a])puts("no");
               else puts("yes");
    }
    return 0;
}
```