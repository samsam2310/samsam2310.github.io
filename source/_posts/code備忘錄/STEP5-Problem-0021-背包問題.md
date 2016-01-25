---
title: 'STEP5::Problem 0021 : 背包問題'
tags:
  - C++
  - Step5
category:
  - Code 備忘錄
  - Greedy
date: 2014-02-25 06:00:02
---


http://web2.ck.tp.edu.tw/~step5/probdisp.php?pid=0021
一堆東西塞到兩個背包使其分別的平均值和最小。

<!--more-->

Greedy，因為大背包平均一定比小背包平均 "除得多"，所以把大的塞到大背包，小的塞到小背包，要注意的是這題要求依字典序輸出，所以要盡量把東西塞到背包1裡。



``` c++
#include <cstdio>
#include <cstdlib>
#include <iostream>
#include <queue>
#define N 100001
using namespace std;
struct MP{int p,c;};
inline bool operator<(const MP &a,const MP &b){
       return (a.c==b.c)? ((a.p<b.p)? true:false):((a.c<b.c)? true:false);
}
int main(int argc, char *argv[])
{
    int t,n,m,tmp,L,pu[N];
    scanf("%d",&t);
    while(t--){
             priority_queue<MP> ST,Fg,Sg;
             scanf("%d %d",&n,&m);
             for(int i=0;i<m+n;i++){
                     scanf("%d",&tmp);
                     ST.push((MP){i,tmp});
             }
             if(n == m){
                     for(int i=0;i<n;i++)printf("1");
                     for(int i=0;i<m;i++)printf("2");
                     puts("");
                     goto ew;
             }
             tmp=(m>n)? m:n;
             /*for(int i=0;i<tmp;i++){
                     L=(n>m)? 1:2;
                     pu[ST.top().p]=L;
                     ST.pop();
             }*/
             for(int i=0;i<tmp;i++){
                     if(n>m){Fg.push(ST.top());}
                     else {Sg.push(ST.top());}
                     ST.pop();
             }
             for(int i=0;i<(m+n-tmp);i++){
                     if(n<=m){Fg.push(ST.top());}
                     else {Sg.push(ST.top());}
                     ST.pop();
             }
             while(!Fg.empty()&&!Sg.empty()){
                     if(Fg.top().c>Sg.top().c){pu[Fg.top().p]=1;Fg.pop();}
                     else if(Fg.top().c<Sg.top().c){pu[Sg.top().p]=2;Sg.pop();}
                     else{
                          if(Fg.top().p<(Sg.top().p)){pu[Sg.top().p]=2;Sg.pop();}
                          else {
                               pu[Fg.top().p]=2;Fg.pop();
                               Fg.push(Sg.top());Sg.pop();
                          }
                     }
             }
             while(!Fg.empty()){pu[Fg.top().p]=1;Fg.pop();}
             while(!Sg.empty()){pu[Sg.top().p]=2;Sg.pop();}
             for(int i=0;i<m+n;i++)
                     printf("%d",pu[i]);
             puts("");
             ew:;
    }
    //system("pause");
    return 0;
} 
```