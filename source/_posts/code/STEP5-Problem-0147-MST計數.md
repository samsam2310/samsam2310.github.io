---
title: 'STEP5::Problem 0147 : MST計數'
tags:
  - C++
  - Stpe5
  - HOJ
  - MST
  - DFS
category:
  - Code 備忘錄
  - Graph
date: 2014-03-27 05:52:54
---



http://web2.ck.tp.edu.tw/~step5/probdisp.php?pid=0147
http://hoj.twbbs.org.tw/judge/problem/view/215
MST計數，學了好久。

<!--more-->

這題是經典題的樣子。
用Kruskal 做生成樹時，會從小挑到大。有幾個性質，第一就是把邊按權重分組，要是同權重只有一條，那只有一種情況，如果有很多條，只要把它加入不會產生環，那他就是一種可能。第二，我現在不管選了誰都不影響後面的結果，第三，同權重的邊選的個數一定相同。

首先因為MST是權重最小的生成樹，所以第三點會成立(要是可以選更多或更少，權重會不一樣)，再來第二點，如果我在權重是1時可以把ABC連起來，權重是2時可以把D也連起來，那不可能存在一條權重是1的邊可以把D跟ABC連起來(不然權重就更小了)，所以我現在怎麼連都不影響後面。第一不解釋。

根據以上幾點，做法就是，先做kruskal，做到權重變大時，就先暫停(先處理權重一樣的邊)，因為題目說同權重的邊不超過十條，所以每舉2的 邊數 次方種可能(我是用位元去處理)，每次都把併查集還原成上一階段，重新加邊進去，如果這種狀態家的邊跟原本做kruskal時加進去的邊一樣，這就是一種合法狀態，一直重複檢查(每一組邊一定有一種，因為一開始做過)。
做完所有組邊之後，每組邊的可能組數相乘就能得到MST個數。

要注意併查集的狀態紀錄，我因為這樣WA了好幾次。

網路上有更詳細的證明，我太廢了寫不出來.........。



``` c++
#include <cstdlib>
#include <iostream>
#include <algorithm>
#define LL long long 
#define N 101
#define M 1001
#define F(n) Fi(i,n)
#define Fi(i,n) Fl(i,0,n)
#define Fl(i,l,n) for(int i=l;i<(n);i++)
using namespace std;
struct MP{
    int a,b,c;
}D[M];
inline bool operator<(const MP a,const MP b)
{
    return a.c<b.c;
}
int F[N],F1[N],F2[N];
int find(int F[],int a){
    while(F[a]!=F[F[a]])F[a]=F[F[a]];
    return F[a];
}
void link(int F[],int a,int b){
    F[find(F,b)]=find(F,a);
}
int main(int argc,char *argv[])
{
    ios_base::sync_with_stdio(false);
    int t,n,m,k;
    cin>>t;
    while(t--){
        cin>>n>>m>>k;
        F(N)F[i]=F2[i]=i;
        F(m)cin>>D[i].a>>D[i].b>>D[i].c;
        sort(D,D+m);
        int l=0,en;
        LL ans=1;
        F(m){
            Fi(j,N)F1[j]=F[j]=F2[j];
            en=0;
            while(i<m&&D[i].c==D[l].c){
                if(find(F,D[i].a)!=find(F,D[i].b)){
                    link(F,D[i].a,D[i].b);
                    en++;
                    n--;
                }
                i++;
            }
            int p=0;
            int TT=(1<<(i-l));
            Fi(j,N)F2[j]=F[j];
            Fi(j,TT){
                Fi(h,N)F[h]=F1[h];
                int tmp;
                while(j<TT){
                    tmp=0;
                    Fi(h,i-l){
                        if(j&(1<<h))tmp++;
                    }
                    if(tmp==en)break;
                    j++;
                }
                if(j>=TT)break;
                tmp=0;
                Fl(h,l,i){
                    if((j&(1<<(h-l)))&&find(F,D[h].a)!=find(F,D[h].b)){
                        link(F,D[h].a,D[h].b);
                        tmp++;
                    }
                }
                if(tmp==en)p++;
                //cout<<'d'<<tn<<' '<<nown<<' '<<tmpn<<endl;
            }
            ans*=(LL)p;
            ans%=(LL)k;
            l=i--;
        }
        if(n==1)cout<<ans<<endl;
        else cout<<"-1\n";
    }
    return 0;
}
```