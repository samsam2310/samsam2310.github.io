---
title: 'HOJ::Problem : 79 - 滅人器'
tags:
  - C++
  - HOJ
  - Tree
category:
  - Code 備忘錄
  - DP
date: 2015-02-08 08:59:35
---


http://hoj.twbbs.org/judge/problem/view/79

需要一些小證明。

<!--more-->

題目是給你一棵樹，還有一個滅人器的條件，包括距離K和負載量S，樹上的每個節點都必須要至少被一個滅人器覆蓋，被覆蓋也就是他要在某個滅人器的距離K內，而且那個滅人器的負載量不超過S。題目問最少需要幾個滅人器。

參考了[這篇](http://cbdcoding.blogspot.tw/2015/01/hoj-79poi-16-stage-1.html)~~

首先，你可以先把S忽略，重點在距離，先把滅人器拆成距離0~距離K，紀錄兩個東西，樹上的每個點的子樹需要多少滅人器和可以提供多少滅人器。

一開始每個點可以提供的滅人器都是0，需要一個距離0的滅人器，然後轉移，父節點除了自己的初始值之外，子節點需要一個距離0的滅人器，等於父節點需要一個距離1的滅人器，子節點提供一個距離1的滅人器，等於父節點提供一個距離0的滅人器。

然後當某個點需要距離K的滅人器時，你不得不在那個點放滅人器了，否則你一定無法覆蓋到某些點，放的數量剛好是S的倍數（大於需要量中最小的），然後開始抵消，因為運算順序的關係，我們可以不用K^2去檢查，只要抵消一樣距離，和距離差1的就好（見code）。

最後，特例就是根節點，在全部抵消完之後，把需要的距離0到距離K加一加取需要幾個滅人器，然後求到答案，注意在過程中供給的陣列可能會爆int，但是實際需要的最大值只要N（點數，最大10^5），所以超過直接壓到N就好。



``` c++
#include <iostream>
#include <vector>
#define F(n) Fi(i,n)
#define Fi(i,n) for(int i=0;i<n;i++)
#define N 100001
#define K 21
#define min(x,y) ((x)<(y)?(x):(y))
using namespace std;
vector<int> G[N];
int n,k,s,in[N][K],out[N][K],mi;
int DFS(int now,int no){
    int cnt=0;
    for(int p:G[now])if(p!=no){
        cnt+=DFS(p,now);
        F(k){
            in[now][i+1]+=in[p][i];
            out[now][i]+=out[p][i+1];
            out[now][i]=(out[now][i]>n)?n:out[now][i];
        }
    }
    in[now][0]=1;
    out[now][k]=(in[now][k]+s-1)/s*s;
    cnt+=(in[now][k]+s-1)/s;
    F(k+1)mi=min(out[now][i],in[now][i]),
          out[now][i]-=mi,in[now][i]-=mi;
    F(k)mi=min(out[now][i+1],in[now][i]),
          out[now][i+1]-=mi,in[now][i]-=mi;
    return cnt;
}
main(){
    ios_base::sync_with_stdio(false);
    cin>>n>>s>>k;
    int a,b,cnt=0;
    F(n-1){
        cin>>a>>b;
        G[a-1].push_back(b-1);
        G[b-1].push_back(a-1);
    }
    cnt=DFS(0,-1);
    F(k+1)Fi(j,i+1)mi=min(out[0][i],in[0][j]),
           out[0][i]-=mi,in[0][j]-=mi;
    F(k)in[0][i+1]+=in[0][i];
    cnt+=(in[0][k]+s-1)/s;
    cout<<cnt<<endl;
}
```