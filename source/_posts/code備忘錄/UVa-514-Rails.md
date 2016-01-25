---
title: 'UVa::514 - Rails'
tags:
  - C++
  - UVa
  - Stack
category:
  - Code 備忘錄
  - Data Structure
date: 2014-02-23 05:48:57
---

http://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=24&page=show_problem&problem=455
stack的應用，先讓一節車廂進來，比對看看出去的誰第一個出去，如果就是stack的TOP就POP掉，重複POP到出去的不是TOP為止，再讓下一節車廂進來。

<!--more-->

最後要是stack有剩下元素沒POP，這個組合就是no，反之yes。



``` c++
#include <cstdio>
#include <cstdlib>
#include <iostream>
#define N 1001
using namespace std;
int ST[N],st=0,T[N],t=0;
int main(int argc,char *argv[])
{
    int tt,a;
    while(1){
        scanf("%d",&a);
        if(a==0)break;
        while(1){
            st=t=0;
            scanf("%d",&T[0]);
            if(T[0]==0){
                puts("");
                break;
            }
            for(int i=1;i<a;i++)scanf("%d",&T[i]);
            for(int i=1;i<=a;i++){
                ST[st++]=i;
                //printf("%d ",i);
                while(ST[st-1]==T[t]){
                    st--;
                    t++;
                    //printf("%d ",ST[st]);
                    if(st==0)break;
                }
            }
            if(st>0)puts("No");
            else puts("Yes");
            //printf("%d\n",t);
        }
    }
    return 0;
}
```