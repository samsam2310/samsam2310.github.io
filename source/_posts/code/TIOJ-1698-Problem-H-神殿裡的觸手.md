---
title: 'TIOJ::1698 . Problem H 神殿裡的觸手'
tags:
  - C++
  - TIOJ
  - Tarjan
category:
  - Code 備忘錄
  - Graph
date: 2015-03-18 09:34:43
---


http://tioj.ck.tp.edu.tw/problems/1698
圖論題，有趣的題目，雖然感覺做起來很煩.....但是其實也還好。

<!--more-->

題目是你有一張圖，你要對他們做最小生成樹MST（或是生成森林），但是有些邊不一定會被加到MST裡，因為權重一樣所以也許可以取代（比如**一個權重都是k的三角形**）。題目問**哪些邊一定會在MST裡**。

解法其實滿漂亮的。
我們會發現，會有多種MST代表一定有兩條以上的邊權重一樣。所以我們可以在做Kruskal's Algorithm的時候，每舉權重一樣的邊加進MST裡。

當然不可能是這樣啦~~~。

不過我們會發現，Kruskal's Algorithm做的事是合併一些MST成為新的MST，所以如果我們把**MST當作一個點**，**這些權重一樣的邊**和這些點就可以**建出一張圖**，一定在最終的MST裡的邊就是這張圖的**橋**！！！！

所以就先做Kruskal's Algorithm，一次處理權重一樣的邊（因為排序過，所以會在一個連續的區間），每次先不要把這些邊拿去合併，先建圖，找橋，把橋標記一下，然後再合併MST繼續Kruskal's Algorithm。

找橋的時候要注意**重邊**，其實可以開一個bool陣列紀錄這條邊用過沒有，建圖用邊去建，**一條邊只能走一次**。比較有問題的是dfs id和low還有建圖的vector這些跟點編號有關係的變數，要是直接初始化複雜度會掉回N^2，不過我們可以在建圖之前，先跑一次所有邊，把所有會用到的點的**相關變數初始化**。

真心覺得找橋的Tarjan比找割點的好寫多了QQ。



``` c++
#include <cstdio>
#include <algorithm>
#include <cstring>
#define F(n) Fi(i,n)
#define Fi(i,n) Fl(i,0,n)
#define Fl(i,l,n) for(int i=l;i<n;i++)
#define N 100010
using namespace std;
struct e{
    int i,a,b,c;
}E[4*N];
int F[N],dfs[N],low[N],EE[4*N],dfsn,ans;
bool ANS[4*N],wed[4*N];
vector<int>G[N];
int find(int x){
    return x==F[x]?x:F[x]=find(F[x]);
}
void Tarjan(int now){
    // printf("GG %d %d\n",now,G[now].size());
    dfs[now]=low[now]=++dfsn;
    for(int p:G[now])if(!wed[p]){
        wed[p]=1;
        int t=EE[p]-now;
        if(dfs[t])low[now]=min(low[now],dfs[t]);
        else{
            Tarjan(t);
            low[now]=min(low[now],low[t]);
            if(low[t]>dfs[now])ANS[p]=1,ans++;
        }
    }
}
int main(){
    int n,m;
    scanf("%d%d",&n,&m);
    F(m)scanf("%d%d%d",&E[i].a,&E[i].b,&E[i].c),E[i].i=i;
    sort(E,E+m,[](const e&a,const e&b){return a.c<b.c;});
    F(n)F[i+1]=i+1;
    F(m){
        int k=E[i].c,p=i;
        while(p<m&&E[p].c==k){
            int a=find(E[p].a),b=find(E[p].b);
            if(a==b){
                wed[E[p++].i]=1;
                continue;
            }
            G[a].push_back(E[p].i);
            G[b].push_back(E[p].i);
            EE[E[p++].i]=a+b;

        }
        Fl(j,i,p)if(!wed[E[j].i])
            Tarjan(F[E[j].a]);
        Fl(j,i,p){
            int a=F[E[j].a],b=F[E[j].b];
            dfs[a]=dfs[b]=0;
            G[a].clear();
            G[b].clear();
        }
        Fl(j,i,p)F[find(E[j].a)]=find(E[j].b);
        i=p-1;
    }
    printf("%d\n",ans);
    if(ans){
        F(m)if(ANS[i])printf("%d ",i+1);
    }
    else printf("0");
    puts("");
    scanf("%*d");
}
```