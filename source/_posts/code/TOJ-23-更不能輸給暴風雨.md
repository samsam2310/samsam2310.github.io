---
title: 'TOJ::23 / 更不能輸給暴風雨'
tags:
  - C++
  - TOJ
  - Stack
category:
  - Code 備忘錄
  - Data Structure
date: 2014-03-03 02:59:02
---


http://2014.sprout.csie.org/oj/pro/23/
一開始覺得很難，但其實是水題.....

<!--more-->

基本上是一個stack的改良版(http://ck10211302.blogspot.tw/2014/02/uva514-rails.html)，但是題目沒有要求最佳解法，所以重複的移入移出遲早會成功。會了上面那題這題就更簡單了。



``` c++
#include <cstdio>
#include <cstdlib>
#include <iostream>
#include <queue>
#define N 5001
using namespace std;
int ST[N],ST2[N],st=0,st2=0;
queue<int> qq;
void push_train();
void move_station_1_to_2();
void move_station_2_to_1();
void pop_train();
void solve(int n,int T[]){
    int t;
    st=t=st2=0;
    for(int i=1;i<=n;i++){
        ST[st++]=i;
        push_train();//qq.push(1);
        while(ST[st-1]==T[t]){
            st--;
            t++;
            move_station_1_to_2();//qq.push(2);
            pop_train();//qq.push(4);
            if(st==0)break;
        }
    }
    while(st!=0||st2!=0){
            //if(tmp==st){no_solution();break;}
            //else tmp=st;
            while(st>0){
                ST2[st2++]=ST[--st];
                move_station_1_to_2();//qq.push(2);
                while(ST2[st2-1]==T[t]){
                    st2--;
                    t++;
                    pop_train();//qq.push(4);
                    if(st2==0)break;
                }
                if(st>0)
                    while(ST[st-1]==T[t]){
                    st--;
                    t++;
                    move_station_1_to_2();//qq.push(2);
                    pop_train();//qq.push(4);
                    if(st==0)break;
                    }
            }
            while(st2>0){
                ST[st++]=ST2[--st2];
                move_station_2_to_1();//qq.push(2);
                while(ST[st-1]==T[t]){
                    st--;
                    t++;
                    move_station_1_to_2();
                    pop_train();//qq.push(4);
                    if(st==0)break;
                }
                if(st2>0)
                    while(ST2[st2-1]==T[t]){
                        st2--;
                        t++;
                        pop_train();//qq.push(4);
                        if(st2==0)break;
                    }
            }
    }
}
```