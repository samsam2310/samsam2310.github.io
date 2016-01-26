---
title: 'TIOJ::1062 . 用餐地點(Lunch)'
tags:
  - C++
  - TIOJ
  - 北市賽
category:
  - Code 備忘錄
  - Math
date: 2014-12-10 08:33:14
---


http://tioj.ck.tp.edu.tw/problems/1062

這題超水的....。

<!--more-->

本來，想了一堆解法，甚麼N^4的都出來了，結果根本就是除了輸入N^2之外O(N)就做完的水題阿....。

這題最難的就是題目，真的看不懂他再寫甚麼。
題目應該是說，有一群人要吃午餐，我們要找一個點設置午餐，而人們移動到相鄰的格子要花費一單位時間，求一個點讓大家花的時間最少，注意是大家花的時間，所以就是所有人的時間加起來。
基本上就是每個格子到午餐的曼哈頓距離乘上該格的人數。不過有個很基本的性質，首先是XY軸可以分開看，不影響。再來就是，只要中位數在的那格，一定是最佳解（把總人數加起來，找出中位數，讓一半的人再前面，一半再後面）。
所以我們先把每一行加起來，找出中位數再哪一格，再把每一列加起來，一樣找出中位數，就可以知道X Y座標了。

然後這題是1base。

[進階版](/code/TIOJ-1242-午餐大反攻-Lunch-Reloaded/)。



``` c++
#include <cstdio>
#define F(n) Fi(i,n)
#define Fi(i,n) Fl(i,0,n)
#define Fl(i,l,n) for(int i=l;i<n;i++)
int X[100],Y[100],a,b,m,n,tmp,sum;
main(){
    scanf("%d%d",&n,&m);
    F(n)Fi(j,m)scanf("%d",&a),X[i]+=a,Y[j]+=a,sum+=a;
    sum=(sum+1)>>1;
    F(n){
        if(tmp+X[i]<sum)tmp+=X[i];
        else{
            printf("%d ",i+1);
            break;
        }
    }
    tmp=0;
    F(m){
        if(tmp+Y[i]<sum)tmp+=Y[i];
        else{
            printf("%d\n",i+1);
            break;
        }
    }
    scanf("%*d");
}
```