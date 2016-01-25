---
title: 'HOJ::Problem : 9 - 大麥克的世界旅行'
tags:
  - C++
  - HOJ
  - Shortest Path
category:
  - Code 備忘錄
  - Graph
date: 2014-03-27 05:48:11
---


http://hoj.twbbs.org.tw/judge/problem/view/9
最短路的變形。

<!--more-->

把匯率想成單向路徑，用Bellman-Ford找最短路，只是路徑是用乘的，如果有負環要輸出0。
因為這題條件有每個點都連通，所以只要判斷負環就好，這題如果用SPFA可以更快而且不會炸double，但是我懶得寫，所以一開始double就溢位了...，改long double就沒事了。



``` c++
#include <cstdlib>
#include <iostream>
#include <iomanip>
#define N 2001
#define M 25001
#define MAX 1e21
#define F(n) Fi(i,n)
#define Fi(i,n) Fl(i,0,n)
#define Fl(i,l,n) for(int i=l;i<n;i++)
using namespace std;
struct MP{
    int a,b;
    long double c;
}E[M];
long double V[N];
int main(int argc,char *argv[])
{
    ios_base::sync_with_stdio(false);
    //cout<<fixed<<setprecision(0);
    int n,m,a,b;
    long double v;
    cin>>n>>m>>v>>a>>b;
    fill(V,V+N,MAX);
    V[a]=v;
    F(m){
        cin>>E[i].a>>E[i].b>>E[i].c;
    }
    bool tag=false;
    F(n){
        Fi(j,m){
            //cout<<i<<' ';
            if(V[E[j].a]*E[j].c<V[E[j].b]){
                V[E[j].b]=V[E[j].a]*E[j].c;
                if(i==n-1)tag=true;
            }
        }
    }
    //F(n+1)cout<<V[i]<<' ';cout<<endl;
    if(tag)cout<<"0\n";
    else cout<<(long long)(V[b]+0.5)<<endl;
    //system("pause");
    return 0;
}
```