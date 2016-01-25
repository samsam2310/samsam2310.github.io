---
title: 'UVa::10954 - Add All'
tags:
  - C++
  - UVa
  - Heap
category:
  - Code 備忘錄
  - Data Structure
date: 2014-02-27 08:39:02
---


http://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=24&page=show_problem&problem=1895
簡單的題目。
加法，但是每加一次就會有個cost=目前總和，也就是要從現在最小的兩個數字開始一個一個加起來。

<!--more-->

用priority_queue，先拿兩個出來，加起來，加到cost裡，如果還有數字沒加，就把這個sum丟回priority_queuq裡，直到queue空了為止(注意最後一次加法完不要丟回去)，因為priority_queue是最大堆疊樹，push前先加個負號，拿出來要記得改回來。



``` c++
#include <cstdio>
#include <cstdlib>
#include <iostream>
#include <queue>
#define LL long long
using namespace std;
int main(int argc,char *argv[])
{
    LL n,tmp,sum,cost;
    while(scanf("%lld",&n)){
            if(n==0)break;
            priority_queue<int> qq;
            for(int i=0;i<n;i++){
                    scanf("%lld",&tmp);
                    qq.push(-tmp);
            }
            cost=0;
            while(!qq.empty()){
                    //printf("%d\n",qq.top());
                    sum=-qq.top();
                    qq.pop();
                    sum+=-qq.top();
                    qq.pop();
                    cost+=sum;
                    if(!qq.empty())qq.push(-sum);
                    //printf("%lld\n",cost);
            }
            printf("%lld\n",cost);
    }
    return 0;
}
```