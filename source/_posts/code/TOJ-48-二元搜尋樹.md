---
title: 'TOJ::48 / 二元搜尋樹'
tags:
  - C++
  - TOJ
  - DFS
  - Tree
category:
  - Code 備忘錄
  - Graph
date: 2014-03-03 03:01:53
---


http://2014.sprout.csie.org/oj/pro/48/
一題經典題吧......

<!--more-->

樹的前序、中序、後續，是遞迴時印出走到的點的順序，前序是先印根節點，中序是先左節點再根節點，後續是先左節點在右節點在根節點。
因為二元搜尋樹的中序排序剛好就是數字從小排到大，利用題目給的前序(按照順序給出根節點)可以找到中序排序中根節點的位置，切開後再遞迴下去跑，順便後序印出解答。



``` c++
#include <cstdlib>
#include <iostream>
#include <algorithm>
#define N 2001
using namespace std;
int T[N],S[N],root;
void backwalk(int le,int re){
    if(le>re)return;
    int i;
    for(i=le;i<=re;i++)if(T[i]==S[root])break;
    //printf("EE:%d %d -> %d(%d)\n",le,re,S[root],root);
    root++;
    backwalk(le,i-1);
    backwalk(i+1,re);
    cout<<T[i]<<endl;
}
int main(int argc,char *argv[])
{
    ios_base::sync_with_stdio(false);
    int tmp;
    int st=0;
    while(cin>>tmp){
        //if(tmp==0)break;
        S[st]=tmp;
        T[st++]=tmp;
    }
    sort(T,T+st);
    //for(int i=0;i<st;i++)printf("%d ",T[i]);
    system("pause");
    root=0;
    backwalk(0,st-1);
    system("pause");
    return 0;
}
```