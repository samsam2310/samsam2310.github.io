---
title: 'STEP5::Problem 0134 : 景點問題'
tags:
  - C++
  - Step5
  - LCA
category:
  - Code 備忘錄
  - Graph
date: 2014-02-20 05:36:49
---


http://web2.ck.tp.edu.tw/~step5/probdisp.php?pid=0134
很有趣(?!)的題目。
只要找出LAC就可以了。第一次寫LAC所以寫得很難看。

<!--more-->

題目有說兩點只有一條路徑-->這是一棵樹。找出兩點的路徑長就是a點到root+b點到root-2*LAC到root。(好像也可以直接從兩點回朔找LCA---Online LCA Sparse Table，不過我那時候不知道ㄏ.....)
至於怎麼做就是把樹壓扁成陣列，再用線段樹搜區間內深度最小的那點(就LAC啦)。不過因為不用更新，所以可以用Sparsr Table寫，O(n lg n+q)。(下面是用BIT寫的)
(再)至於怎麼壓扁要用時間標記，把DFS的時候進出的順序記錄下來。


``` c++
#include <cstdlib>
#include <cstdio>
#include <iostream>
#include <vector>
#define N 30002
using namespace std;
vector<int> T[N];
int A[2*N],F[N],H[2*N],BT[8*N];
int Buildtree(int le,int re,int id)
{
     if(le==re){BT[id]=A[le];return A[le];}
     int tmp1,tmp2;
     tmp1=Buildtree(le,(le+re)/2,id*2);
     tmp2=Buildtree((le+re)/2+1,re,id*2+1);
     BT[id]= (H[tmp1]<H[tmp2])? tmp1:tmp2;
     //printf("Q %d,%d:%d\n",le,re,A[le]);
     return BT[id];
}
int find(int a,int b,int le,int re,int id)
{
    if(re==le)return BT[id];
    if(re<a||le>b)return -1;
    if(a<=le&&re<=b)return BT[id];
    
    int tmp=(re+le)/2,tmp1,tmp2;
    if(le==re) 
               return BT[id];
    tmp1=find(a,b,le,tmp,id*2);
    tmp2=find(a,b,tmp+1,re,id*2+1);
    //printf("YYY:%d:%d %d:%d\n",le,tmp1,re,tmp2);
    if(tmp1!=-1 && tmp2!=-1)
               return (H[tmp1] < H[tmp2])? tmp1:tmp2;
    else 
               return (tmp1!=-1)? tmp1:tmp2;
}
int DFS(int now,int no,int dr,int tm)
{
    //printf("%d ",tm);
    A[tm]=now;
    F[now]=tm;
    H[tm]=dr;
    for(int i=0;i*2<T[now].size();i++)
            if(T[now][i*2] != no){
                        tm += 1;
                        tm = DFS(T[now][i*2],now,dr+T[now][i*2+1],tm);
                        tm = tm+1;
                        //printf("%d ",tm);
                        A[tm]=now;
                        H[tm]=dr;
            }
    return tm;
}
int main(int argc,char *argv[])
{
    int n;
    scanf("%d",&n);
    int a,b,c;
    for(int i=1;i<n;i++){
            scanf("%d %d %d",&a,&b,&c);
            a++;b++;
            T[a].push_back(b);
            T[a].push_back(c);
            T[b].push_back(a);
            T[b].push_back(c);
    }
    int tm=1;
    tm = DFS(1,-1,0,tm);
    Buildtree(1,tm,1);
    int q,now=1,qw,sum=0;
    scanf("%d",&q);
    for(int i=0;i<q;i++){
            scanf("%d",&qw);
            qw++;
            a=(F[now] <= F[qw])? F[now]:F[qw];
            b=(F[now] > F[qw])? F[now]:F[qw];
            c=find(a,b,1,tm-1,1);
            sum+=H[a]+H[b]-H[c]-H[c];
            //printf("OO:%d(%d %d %d %d) \n",H[a]+H[b]-H[c]-H[c],a,b,H[c],sum);
            now=qw;
    }
    printf("%d\n",sum);
    //system("pause");
    return 0;
}
```