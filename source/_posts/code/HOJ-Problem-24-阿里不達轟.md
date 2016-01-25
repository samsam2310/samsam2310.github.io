---
title: 'HOJ::Problem : 24 - 阿里不達轟'
tags:
  - C++
  - HOJ
  - NPSC
  - DFS
category:
  - Code 備忘錄
  - Search
date: 2014-03-13 04:58:25
---


http://hoj.twbbs.org.tw/judge/problem/view/24
這題是NPSC國中組出賽題。給你炸彈的座標、半徑和敵人的座標、半徑，只要兩圓有碰到就算攻擊的到敵人，問最少要幾顆炸彈。

<!--more-->

這題因為範圍很小，直接暴搜就好。判斷兩圓有沒有碰到的方法，我是用兩點的距離平方(直角三角形)和半徑和的平方做比較。
我用一個整數紀錄一個炸彈可以炸到哪幾個敵人(2進位一位代表一個敵人)，DFS時就用OR運算，只要全部都炸的到就回傳炸彈數(要多少才算全部炸到我先預處理了)。
要是跑完了都炸不到就輸出Impossible。



``` c++
#include <cstdlib>
#include <iostream>
#define M 20
#define N 10
using namespace std;
struct MP{
    int x,y,r;
}B[M],E[N];
int R[M],dl[N];
inline bool operator>(const MP a,const MP b)
{
    return (a.x-b.x)*(a.x-b.x)+(a.y-b.y)*(a.y-b.y)<=(a.r+b.r)*(a.r+b.r);
}
int DFS(int m,int sum,int b,int dl,int dm)
{
    //cout<<m<<' '<<sum<<' '<<b<<' '<<dl<<' '<<dm<<endl;
    if(m>=dm)return 100;
    int tmp1=DFS(m+1,sum,b,dl,dm);
    sum|=R[m];
    if(sum==dl)return b+1;
    int tmp2=DFS(m+1,sum,b+1,dl,dm);
    return (tmp1<tmp2)? tmp1:tmp2;
}
int main(int argc,char *argv[])
{
    ios_base::sync_with_stdio(false);
    for(int i=0;i<N-1;i++)dl[i+1]=dl[i]+(1<<i);
    int t,m,n;
    cin>>t;
    while(t--){
            cin>>m>>n;
            fill(R,R+M,0);
            for(int i=0;i<m;i++)cin>>B[i].x>>B[i].y>>B[i].r;
            for(int i=0;i<n;i++)cin>>E[i].x>>E[i].y>>E[i].r;
            for(int i=0;i<m;i++)
                    for(int j=0;j<n;j++){
                            if(B[i]>E[j])R[i]|=1<<j;
                    }
            int tmp1=DFS(0,0,0,dl[n],m);
            if(tmp1>M)cout<<"Impossible\n";
            else cout<<tmp1<<endl;
    }
    //system("pause");
    return 0;
}
```