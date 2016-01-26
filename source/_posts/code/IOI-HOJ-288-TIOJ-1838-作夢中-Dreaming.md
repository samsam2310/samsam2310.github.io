---
title: 'IOI::HOJ-288::TIOJ-1838::作夢中 Dreaming'
tags:
  - C++
  - TIOJ
  - IOI
  - HOJ
  - Tree
category:
  - Code 備忘錄
  - Graph
date: 2015-03-18 09:32:04
---


http://tioj.ck.tp.edu.tw/problems/1838
IOI 2013。

<!--more-->

這題WA了好幾次QQ。
題目是你有一堆樹，還有一堆長度是L的邊，你要把這些樹連結成一棵，並且讓**最遠的兩個點最近**，求他們之間的距離。

先觀察一下題目，會發現最遠的兩個點的距離就是最長鍊的長度。最長鍊可以透過先**亂DFS到最遠的點，再從那個點DFS到最遠的點**，這兩個點之間的鍊就是最長鍊。
不太嚴謹的證明：第一次DFS，起點可能不在最長鍊上，但是最遠的點一定是**最長鍊的端點**，不然你一定可以找到一條鍊比最長鍊更長，但那是矛盾的。然後從端點DFS最遠點也一定是另外一個端點。

所以，答案有可能是那堆**樹裡的最長鍊**，不然，就是把這些**樹接起來之後的最長鍊**。

找最長鍊時，順便找每棵樹的「圓心」（以這個點當根，最深的子樹最淺），利用他找出「**半徑**」（以圓心當根，最深的子樹的深度），用題目給的L長度的邊把他們接在一起（把圓心接在一起最好），然後我們就可以發現，最好的接法就是把所有的**樹接到半徑最長的樹上**。

於是最長鍊除了在樹裡之外，還有可能是**最長半徑跟次長半徑加上一條L邊，不然就是次長半徑的樹和第3長半徑加上兩條L邊**（全部都接到最長半徑樹上，如果L很長就會是第2種狀況。）不用考慮其他狀況，因為都不會比這幾種狀況長。

找半徑要好好思考，只要差一點就WA了XDD。



``` c++
#include <cstdio>
#include <vector>
#include <cstring>
#define F(n) Fi(i,n)
#define Fi(i,n) for(int i=0;i<n;i++)
#define N 100010
using namespace std;
struct MP{
    int t,c;
}F[N];
int ans,ST[5];
bool wed[N];
bool gin(int &a){
    char c;
    while(c=getchar(),c<'0'||c>'9')if(c==-1)return 0;
    a=c-'0';
    while(c=getchar(),c>='0'&&c<='9')a=a*10+c-'0';
    return 1;
}
MP DFS(vector<MP>*G,int now,int no){
    wed[now]=1;
    MP drv=(MP){now,0};
    F[now]=drv;
    for(MP p:G[now])if(p.t!=no){
        MP tmp=DFS(G,p.t,now);
        tmp.c+=p.c;
        if(tmp.c>drv.c)drv=tmp,F[now]=p;
    }
    return drv;
}
int main(){
    int n,m,l,a,b,c;
    // while(~scanf("%d%d%d",&n,&m,&l)){
    while(gin(n),gin(m),gin(l)){
        vector<MP>G[N];
        ans=0;
        memset(ST,0,sizeof(ST));
        memset(wed,0,sizeof(wed));
        F(m){
            // scanf("%d%d%d",&a,&b,&c);
            gin(a);gin(b);gin(c);
            G[a].push_back((MP){b,c});
            G[b].push_back((MP){a,c});
        }
        F(n)if(!wed[i]){
            int str=DFS(G,i,-1).t;
            int tmp=DFS(G,str,-1).c;
            ans=max(tmp,ans);
            int sum=0,st=3;
            if(tmp)while(sum+F[str].c<=tmp/2)sum+=F[str].c,str=F[str].t;
            tmp=min(sum+F[str].c,tmp-sum);
            while(st&&tmp>ST[st-1])ST[st]=ST[st-1],st--;
            ST[st]=tmp;
        }
        if(n-m>2)ans=max(ans,max(ST[0]+l+ST[1],ST[1]+ST[2]+l*2));
        else if(n-m>1)ans=max(ans,ST[0]+l+ST[1]);
        printf("%d\n",ans);
    }
}
```