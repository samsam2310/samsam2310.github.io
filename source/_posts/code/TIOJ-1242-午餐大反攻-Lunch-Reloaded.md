---
title: 'TIOJ::1242 . 午餐大反攻 Lunch Reloaded'
tags:
  - C++
  - TIOJ
category:
  - Code 備忘錄
  - Math
date: 2014-12-10 08:31:50
---


http://tioj.ck.tp.edu.tw/problems/1242
這題是[這題](/code/TIOJ-1062-用餐地點-Lunch/)的延伸。

<!--more-->

基本上和之前那題一樣，稍微不同的是，這題範圍變大，而且有更多查尋，而且要輸出代價（每個點的人數乘上到午餐點的曼哈頓距離）。
不過基本上概念不變，先找出X Y的中位數，再暴力FOR回圈求出代價就好。只是前題是，這裡所用到的所有區間和都用前綴和預處理好了。複雜度 O(N^2*q) ，感覺好像很慢，也許是測資爛八XD。
如果還要再更快也許就要對代價那裡做一些預處理吧。



``` c++
#include <iostream>
#define abs(x) ((x)>0?(x):(-(x)))
#define F(n) Fi(i,n)
#define Fi(i,n) Fl(i,0,n)
#define Fl(i,l,n) for(int i=l;i<n;i++)
#define LL long long
using namespace std;
LL D[1001][1001];
int n,m,x1,x2,y1,y2,a;
LL find(int x,int y,int t,bool tag){
    if(tag)return D[t][y]-D[t][x]-D[t-1][y]+D[t-1][x];
    else return D[y][t]-D[x][t]-D[y][t-1]+D[x][t-1];
}
main(){
    ios_base::sync_with_stdio(false);
    cin>>n>>m;
    F(n)Fi(j,m)cin>>D[i+1][j+1],D[i+1][j+1]+=D[i][j+1]+D[i+1][j]-D[i][j];
    cin>>a;
    while(cin>>x1>>x2>>y1>>y2){
        x1--;y1--;
        LL sum=D[x2][y2]-D[x2][y1]-D[x1][y2]+D[x1][y1],tmp=0;
        sum=(sum+1)>>1;
        int tx,ty;
        for(tx=x1+1;tx<=x2&&tmp+find(y1,y2,tx,1)<sum;tx++)tmp+=find(y1,y2,tx,1);
        tmp=0;
        for(ty=y1+1;ty<=y2&&tmp+find(x1,x2,ty,0)<sum;ty++)tmp+=find(x1,x2,ty,0);
        sum=0;
        Fl(i,x1+1,x2+1)sum+=find(y1,y2,i,1)*abs(i-tx);
        // cout<<sum<<endl;sum=0;
        Fl(i,y1+1,y2+1)sum+=find(x1,x2,i,0)*abs(i-ty);
        // cout<<ty<<endl;
        cout<<sum<<endl;
    }
}
```