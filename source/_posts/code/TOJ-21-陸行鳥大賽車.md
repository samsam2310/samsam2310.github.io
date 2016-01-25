---
title: 'TOJ::21 / 陸行鳥大賽車'
tags:
  - C++
  - TOJ
  - Link List
category:
  - Code 備忘錄
  - Data Structure
date: 2014-03-03 02:57:29
---


http://2014.sprout.csie.org/oj/pro/21/
題目要求一個link list，記錄賽車的名次。

<!--more-->

我先建了兩個陣列，F、B分別指向前面、後面，然後只要有人被攻擊，就讓他前面跟後面的人修改值，如果超車也一樣，要注意的是，第一名的車子會換，所以HEAD指到的車也會換，但是一般修改不會改到他。
我的作法是紀錄誰還活著，從還活著的開始往前找到第一名，在輸出。
但其實可以新增一台第0名得車子，輸出時忽略他就好。



``` c++
#include <cstdio>
#include <cstdlib>
#include <iostream>
#define N 100001
using namespace std;
int F[N],B[N];
bool L[N]={0};
void PP(){
    int tmp=1;
    while(L[tmp])tmp++;
    for(int i=F[tmp];i!=-1;i=F[i])tmp=i;
    for(int i=tmp;i!=-1;i=B[i])printf("(%d)%d(%d) ",F[i],i,B[i]);
    puts("");
}
int main(int arc,char *argv[])
{
    int n,q,a,b;
    scanf("%d",&n);
    for(int i=1;i<=n;i++)F[i]=i-1;
    F[1]=-1;
    for(int i=1;i<=n;i++)B[i]=i+1;
    B[n]=-1;
    scanf("%d",&q);
    //puts("aa");
    while(q--){
        scanf("%d %d",&a,&b);
        if(a==0){
            if(B[b]!=-1)F[B[b]]=F[b];
            if(F[b]!=-1)B[F[b]]=B[b];
            F[b]=B[b]=-1;
            L[b]=1;
        }else if(a==1){
            //puts("bb");
            int tmpb=B[b],tmpf=F[b];
            if(tmpf!=-1)B[b]=tmpf;
            if(tmpf!=-1){
                F[b]=F[tmpf];
                if(F[tmpf]!=-1)B[F[tmpf]]=b;
            }
            if(tmpf!=-1)F[tmpf]=b;
            if(tmpf!=-1)B[tmpf]=tmpb;
            if(tmpb!=-1&&tmpf!=-1)F[tmpb]=tmpf;
        }
        //PP();
    }
    //puts("aa");
    int tmp=1;
    while(L[tmp])tmp++;
    for(int i=F[tmp];i!=-1;i=F[i])tmp=i;
    //printf("%d(%d) ",tmp,B[tmp]);
    //for(int i=tmp;i!=-1;i=B[i])printf("(%d)%d(%d) ",F[i],i,B[i]);
    printf("%d",tmp);
    for(int i=B[tmp];i!=-1;i=B[i])printf(" %d",i);
    puts("");
    system("pause");
    return 0;
}
```