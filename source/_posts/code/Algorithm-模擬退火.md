---
title: 'Algorithm::模擬退火'
tags:
  - C++
  - 模擬退火
category:
  - Code 備忘錄
  - Algorithm
date: 2015-03-14 09:15:58
---


模擬退火算是一種搜尋的算法，可以用來解決一些例如座標的搜尋問題。

<!--more-->


模擬退火是這次去枝幹學會的，最基本的例題就是找一點到其他N個點，使他們到該點距離和最短。
這種題目有幾種做法，三分搜、爬山法等等，但是寫完之後覺得模擬退火效率不錯而且不會很難寫。

模擬退火的意思就是模擬金屬冷卻時原子的運動狀況，也許有點像擴散作用吧，就是你給他攪一攪他就會往比較鬆的地方跑，原子會往能量比較低還是什麼的地方鑽....等等。

講到這裡感覺很麻煩，其實做法不難，只要先從一個點開始，寫一個函數判斷某一個座標的值是多少，比如這題就枚舉所有點算出答案，之後定一個適當的轉移距離，並隨機一個方向，看看那個位置的值有沒有比現在好，有的話就走過去，沒有的話，便**縮短距離**，持續收練，慢慢的就會走向正解，可以想像是一顆粒子隨機震動並慢慢停止。

整個演算法的瓶頸應該會落在轉移狀態的運算上，對於時間要求比較緊的題目可以裡用狀態轉移取代重新運算來加速。

比較有趣的是，當題目需要搜尋兩個點的時候，要怎麼做才可以比較容易搜到好解。
初步的實驗，是先震動一個點，之後就把他當成只有一個點的來解，解出來的解是第一個點這樣移動之後的值，如果沒有比較好，則兩個點都回到原位。
實驗了一下發現，第二個點如果不回到原位，會讓解的誤差變大，其實這應該可以推出來，因為之前的最優解是建立在兩個點在某個位置時的解，不讓第二個點規位等於破壞原本的最優解。

感覺比三分搜好寫多了XD，隨機方向可以利用cmath的三角函數，直接random一個整數塞進去就可以了，非常方便，大致上應該夠隨機了。縮短距離可以直接乘0.9，中止條件可以看距離小於多少。



``` c++
//尋找和所有點距離和最小的點
#include <cstdio>
#include <cstdlib>
#include <cmath>
#define F(n) Fi(i,n)
#define Fi(i,n) for(int i=0;i<n;i++)
#define N 1010
using namespace std;
int X[N],Y[N],n;
inline double pow2(double x){
    return x*x;
}
double check(double x,double y){
    double ans=0;
    F(n)ans+=sqrt(pow2(x-X[i])+pow2(y-Y[i]));
    return ans;
}
int main(){
    scanf("%*d");
    while(~scanf("%d",&n)){
        F(n)scanf("%d%d",X+i,Y+i);
        double x=0,y=0,tx,ty,tans,l=10000,ans;
        ans=check(x,y);
        while(l>1e-4){
            int tmp=rand();
            tx=x+l*cos(tmp);ty=y+l*sin(tmp);
            tans=check(tx,ty);
            if(tans<ans)ans=tans,x=tx,y=ty;
            else l*=0.9;
        }
        printf("%.9f\n",2*ans);
    }
}

//尋找兩個點使他們跟給定的四個點最小生成樹最小
#include <cstdio>
#include <cstdlib>
#include <cmath>
#include <algorithm>
#define F(n) Fi(i,n)
#define Fi(i,n) Fl(i,0,n)
#define Fl(i,l,n) for(int i=l;i<n;i++)
#define N 10
using namespace std;
int X[N],Y[N],n,F[N],e;
struct E{
    int a,b;
    double c;
}G[N*2];
struct V{
    double x,y;
    V operator+(double l){
        int tmp=rand();
        return (V){x+l*cos(tmp),y+l*sin(tmp)};
    }
}v[N];
int find(int x){
    return x==F[x]?x:F[x]=find(F[x]);
}
inline double pow2(double x){
    return x*x;
}
double check(V s1,V s2){
    double ans=0;
    e=0;v[4]=s1,v[5]=s2;
    F(5)Fl(j,i+1,6)
        G[e++]=(E){i,j,sqrt(pow2(v[i].x-v[j].x)+pow2(v[i].y-v[j].y))};
    F(6)F[i]=i;
    sort(G,G+e,[](E a,E b){return a.c<b.c;});
    F(e){
        if(find(G[i].a)!=find(G[i].b)){
            ans+=G[i].c;
            F[find(G[i].a)]=find(G[i].b);
        }
    }
    return ans;
}
int main(){
    scanf("%d",&n);
    while(n--){
        F(4)scanf("%lf%lf",&v[i].x,&v[i].y);
        double ttans,tans,ans,l1=10000,l2;
        V s1=(V){0,0},s2=(V){0,0},ts1,ts2,tmp;
        ans=check(s1,s2);
        while(l1>1e-3){
            l2=10000;
            ts1=s1+l1;
            tans=check(ts1,s2);
            tmp=s2;
            while(l2>1e-3){
                ts2=s2+l2;
                ttans=check(ts1,ts2);
                if(ttans<tans)tans=ttans,s2=ts2;
                else l2*=0.9;
            }
            if(tans<ans)ans=tans,s1=ts1;
            else l1*=0.9,s2=tmp;
        }
        printf("%f\n",2*ans);
    }
}
```