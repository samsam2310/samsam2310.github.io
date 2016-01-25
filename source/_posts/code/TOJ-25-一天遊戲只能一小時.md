---
title: 'TOJ::25 / 一天遊戲只能一小時'
tags:
  - C++
  - TOJ
  - Link List
category:
  - Code 備忘錄
  - Data Structure
date: 2014-03-03 03:00:02
---


http://2014.sprout.csie.org/oj/pro/25/
這題有點有趣。題目要排隊，但是不能直接用std::queue，會TLE。

<!--more-->

因為這題有一個操作--整個隊伍接到另外一個隊伍後面，所以要用link list，我用一個陣列紀錄自己後面的人是誰，用一個陣列當作每家店，每家店都會指向第一個排隊和最後一個排隊的人，這樣可以不用每次跑一遍隊伍找最後一個，再來就照著題目交換指標就行了。



``` c++
#include <cstdio>
#include <cstdlib>
#include <iostream>
#define N 1000001
#define M 101
using namespace std;
int L[M][2]={0},O[N]={0};
void PP(int n){
     for(int i=1;i<=n;i++){
            printf("queue(%d %d) %d:",L[i][0],L[i][1],i);
            if(!L[i][0])printf(" empty");
            else for(int j=L[i][0];j!=0;j=O[j])printf(" %d",j);
            puts("");
    }
}
int main(int argc,char *argv[])
{
    int n,m,a,b;
    char c[10];
    scanf("%d %d",&n,&m);
    while(m){
             scanf("%s",c);
             if(c[0]=='A'){
                   scanf("%d %d",&a,&b);
                   if(L[a][0]==0) L[a][0]=L[a][1]=b;
                   else           L[a][1]=O[L[a][1]]=b;
                   O[b]=0;
                   m--;
             }else if(c[0]=='L'){
                   scanf("%d",&a);
                   if(L[a][0]==0)printf("queue %d is empty!\n",a);
                   else  L[a][0]=O[L[a][0]];
                   if(!L[a][0])L[a][1]=0;
                   m--;
             }else if(c[0]=='J'){
                   scanf("%d %d",&a,&b);
                   if(L[b][0]){
                           O[L[b][1]]=L[a][0];
                           if(L[a][1])L[b][1]=L[a][1];
                   }else{
                           L[b][0]=L[a][0];
                           L[b][1]=L[a][1];
                   }
                   L[a][0]=L[a][1]=0;
                   m--;
             }
             //PP(n);
    }
    for(int i=1;i<=n;i++){
            printf("queue %d:",i);
            if(!L[i][0])printf(" empty");
            else for(int j=L[i][0];j!=0;j=O[j])printf(" %d",j);
            puts("");
    }
    system("pause");
    return 0;
}
```