---
title: "TOJ::49 / It's my ____ in the box"
tags:
  - C++
  - TOJ
category:
  - Code 備忘錄
  - Else
date: 2014-03-25 05:32:21
---


http://2014.sprout.csie.org/oj/pro/49/
一堆神奇的箱子。

<!--more-->

一個陣列紀錄放到誰裡面，然後把自己的更新tag改成true。
輸入結束後，重複檢查更新tag，true就更新他指到的箱子，並把他的tag改成true，(更新時要記得先扣掉上次加上去的東西，再加上這次要加的東西)，直到所有tag都變成false。



``` c++
#include <cstdio>
#include <cstdlib>
#include <iostream>
#include <vector>
#define N 100001
using namespace std;
int SUM[N];
bool ch[N];
vector<int> P[N];
int DFS(int now,int no)
{
    //cout<<"DD:"<<now<<endl;
    int sum=1;
    //cout<<"RR:"<<now<<endl;
    for(int i=0;i<P[now].size();i++)
            if(P[now][i]!=no)sum+=DFS(P[now][i],now);
    SUM[now]=sum;
    return sum;
}
int main(int argc,char *argv[])
{
    ios_base::sync_with_stdio(false); 
    int t,n,m,a,b,q,qt;
    cin>>t;
    while(t--){
            cin>>n>>m;
            fill((bool*)ch,(bool*)ch+N,true);
            for(int i=0;i<N;i++)P[i].clear();
            for(int i=0;i<m;i++){
                    cin>>a>>b;
                    P[a].push_back(b);
                    ch[b]=false;
                    //cout<<"TT"<<endl;
            }
            //cout<<P[0].size();
            for(int i=1;i<=n;i++)
                    if(ch[i])SUM[i]=DFS(i,i);
            // for(int i=0;i<=n;i++)
                    // cout<<"node "<<i<<": "<<SUM[i]<<endl;
            cin>>q;
            while(q--){
                    cin>>qt;
                    cout<<SUM[qt]<<endl;
            }
            system("pause");
    }
    return 0;
}
```