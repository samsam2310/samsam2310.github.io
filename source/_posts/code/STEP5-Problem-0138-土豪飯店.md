---
title: 'STEP5::Problem 0138 : 土豪飯店'
tags:
  - C++
  - Step5
  - Shortest Path
category:
  - Code 備忘錄
  - Graph
date: 2014-02-20 05:36:07
---


http://web2.ck.tp.edu.tw/~step5/probdisp.php?pid=0138
題目是給定第a天比第b天最多多c人。
可以將題目轉成有向有權圖。然後Dijkstra's就AC了。O(E log V)

<!--more-->

邊的部分是單向的，因為要是反向權重會變成負的。
然後這題我的priority_queue一直靠邊的權重排列，錯了好幾次。Dijkstra's比較的是該點到起點的距離!!!!!!(而且不能有負邊)
有試過Bellman-Ford，但O(VE)會TLE，不過code不會很難，幾個for就跑完了。

Dijkstra's的code。


``` c++
#include <cstdio>
#include <cstdlib>
#include <iostream>
#include <vector>
#include <queue>
#define LL long long
#define M 250001
#define N 100001
#define QQ qq.top()
using namespace std;
struct MP{int a,b,c;};
vector<int> mp[N];
priority_queue<MP> qq;
LL mm[N];
bool ted[N];
inline bool  operator<(const MP a,const MP b){
    return (a.c>b.c);
}
int main(int argc,char *arv[])
{
    int n,m,s;
    scanf("%d %d %d",&n,&m,&s);
    for(int i=0;i<m;i++){
            int a,b,c;
            scanf("%d %d %d",&a,&b,&c);
            mp[b].push_back(a);
            mp[b].push_back(c);
    }
    for(int i=0;i*2<mp[1].size();i++)
            qq.push((MP){1,mp[1][i*2],s+mp[1][i*2+1]});
    ted[1]=true;
    mm[1]=s;
    while(!qq.empty()){
            mm[qq.top().b]=qq.top().c;
            ted[qq.top().b]=true;
            int tmp=qq.top().b;
            //printf("%d->%d  C:%d\n",qq.top().a,tmp,mm[tmp]);
            qq.pop();
            for(int i=0;i*2<mp[tmp].size();i++)
                    if(!ted[mp[tmp][i*2]]){
                        qq.push((MP){tmp,mp[tmp][i*2],mm[tmp]+mp[tmp][i*2+1]});
                        //printf("EE:%d %d %d\n",tmp,mp[tmp][i*2],mm[tmp]+mp[tmp][i*2+1]);
                    }
            while(!qq.empty())if(ted[qq.top().b])qq.pop();else break;
    }
    for(int i=1;i<=n;i++)printf("%lld ",mm[i]);
    puts("");
    //system("pause");
    return 0;
}
```