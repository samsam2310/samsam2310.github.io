---
title: 'HOJ::Problem : 148 - 海綿寶寶之泡芙阿姨駕訓班(尤拉路徑)'
tags:
  - C++
  - HOJ
  - DFS
category:
  - Code 備忘錄
  - Graph
date: 2014-03-23 05:24:55
---


尤拉路徑。
http://hoj.twbbs.org.tw/judge/problem/view/148

<!--more-->

尤拉路徑是一條路徑，每個邊都走過而且只走一次，有點像一筆畫畫完一張圖。
性質：點連到的邊如果是奇數個，稱為奇點，反之稱為偶點，奇點的數量是2或0就會存在尤拉路徑，其中0的狀況起點終點為任意點，2的狀況這兩點為起點終點。

找尤拉路可以透過DFS，先隨意找一點DFS(如果有奇點要從奇點開始)，並在遞迴函式的return前面加上push現在離開的那個點，照這個順序就能找出尤拉路徑，模擬一次就可以明白整個過程。

順便記一下，雖然這題不用字典序最小，可是如果要字典序的話，先DFS後序的結果反向輸出就是答案了。



``` c++
#include <cstdlib>
#include <iostream>
#include <vector>
#define N 50011
#define M 200011
using namespace std;
vector<int>MAP[N];
int S[M],st=0,E[M];
void DFS(int now,int e)
{
    E[e]=0;
    for(int i=0;i<MAP[now].size();i++){
        if(E[MAP[now][i]]){
            DFS(E[MAP[now][i]]-now,MAP[now][i]);
        }
    }
    S[st++]=now;
    return;
}
int main(int argc,char *argv[])
{
    ios_base::sync_with_stdio(false);
    int m,n,a,b;
    cin>>n>>m;
    for(int i=0;i<m;i++){
        cin>>a>>b;
        E[i]=a+b;
        MAP[a].push_back(i);
        MAP[b].push_back(i);
    }
    int sr=1;
    for(int i=2;i<=n;i++)if(MAP[i].size()%2)sr=i;
    DFS(sr,m+1);
    //cout<<sr<<endl;
    for(int i=1;i<st;i++)cout<<S[i-1]<<' '<<S[i]<<endl;
    //system("pause");
    return 0;
}
```