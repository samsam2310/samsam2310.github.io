---
title: 'HOJ::Problem : 143 - 海綿寶寶之觸手水母田〈二〉(割點)'
tags:
  - C++
  - HOJ
  - Tarjan
category:
  - Code 備忘錄
  - Graph
date: 2014-06-01 08:17:16
---


http://hoj.twbbs.org.tw/judge/problem/view/143
找割點的數量

<!--more-->

基本上和[這題](/code/TOJ-183-高棕櫚傳遞鏈/)一樣，把找點改成記數。



``` c++
#include <cstdlib>
#include <iostream>
#include <vector>
#define N 50001
#define M 200001
#define MM MAP[now][i]
using namespace std;
vector<int>MAP[N];
int L[M],low[N],ti[N],t=0,sum=0;
bool wed[N],IF[N];
int DFS(int now,int root)
{
    ti[now]=t++;
    low[now]=ti[now];
    wed[now]=true;
    int tmpr=0;
    for(int i=0;i<MAP[now].size();i++){
        if(L[MM]!=0){
            int tmp=L[MM]-now;
            L[MM]=0;
            if(!wed[tmp]){
                tmpr++;
                DFS(tmp,root);
                low[now]=(low[now]<low[tmp])? low[now]:low[tmp];
            }else{
                low[now]=(low[now]<ti[tmp])? low[now]:ti[tmp];
            }
            if(low[tmp]>=ti[now]&&now!=root)IF[now]=true;//找割點
        }
    }
    if(now==root&&tmpr>=2)sum++;//割點，根要另外算.有2子節點是割點
    //cout<<now<<" tmp "<<' '<<ti[now]<<' '<<low[now]<<' '<<sum<<endl;
    return sum;
}
int main(int argc,char *argv[])
{
    //ios_base::sync_with_stdio(false);
    int n,m;
    cin>>n>>m;
    for(int i=0;i<m;i++){
        int a,b;
        cin>>a>>b;
        L[i]=a+b;
        MAP[a].push_back(i);
        MAP[b].push_back(i);
    }
    for(int i=1;i<=n;i++)
        if(!wed[i])DFS(i,i);
    for(int i=1;i<=n;i++)if(IF[i])sum++;
    cout<<sum<<endl;
    // for(int i=1;i<=n;i++){
    //  cout<<i<<' '<<low[i]<<' '<<ti[i]<<endl;
    // }
    //system("pause");
    return 0;
}
```