---
title: 'UVa::1479 - Graph and Queries（Treap）'
tags:
  - C++
  - UVa
  - Treep
category:
  - Code 備忘錄
  - Data Structure
date: 2015-02-11 09:01:36
---


http://uva.onlinejudge.org/external/14/1479.html
Treap經典題。第K大的值，修改值，合併Treap。

<!--more-->

這題題目是給你一張無向圖，有三種操作，查詢跟點 i 連接的所有點中，第K大的值、修改某個點的值，刪掉某條邊。最後輸出所有查詢的平均值。

這題要**離線逆向**回去做，先把操作吃進來，修改值先改陣列然後紀錄原本是甚麼（等等才可以改回去），刪掉邊就先把這個邊標記掉，查尋直接記起來。

這樣問題就變成，有一些連通塊會合併，這樣就可以用**Treap**做了。

Treap，樹堆，Tree 跟 heap的合體。網路上查到的大部分的大陸人都是用旋轉的方式來進行操作，但是其實有更簡單的方法。
定義兩個操作，**合併跟分割（不用旋轉）**，合併只能合併兩個Key值可以分成兩堆的treap（左邊都比較小，右邊都比較大，才可以合併，這是定義也是性質），分割是把Key小於K的跟大於K的割開來。的分割和合併是對稱的，一個Treap可以分割再合併，他會變回原來的Treap。
透過這兩個操作可以做到幾乎所有的Treap操作，插入點就是先把K-1割下來，把他跟新點合併，然後再合回去，刪除就是把小於K跟大於K的割開，把中間刪掉再合回去，複雜度都是O（logn），重點是很好寫~~，要持久化也比較簡單。只比用旋轉慢一點點。

因為剛剛已經把操作都逆向處理好了，建立N個Treap，把沒被標記掉的邊拿出來，用desjoin set查詢兩個點是不是再一個連通塊裡，是的話就不用合併了（會悲劇喔），不是的話，就合併（一開始合併完沒更新desjoin set，RE好久...），然後依序操作，刪邊變成加邊，可以直接合併（**不是上面說的合併**，見下面說明）Treap，修改點就是刪掉點在加點（注意這裡我WA好久QQ，因為會有一樣的點，所以把小於K的切下來，大於K的切下來之後，可能會剩很多**大小一樣的點，只能刪一個！！！！**，比較好寫的作法是依序把 小於K的,等於K的左子樹，等於K的右子樹,大於K合併，至於為甚麼應該很明顯。你刪掉了 等於K的那棵Treap的root了~~）。
查詢就是只要知道有K-1個數字比他大的數字是誰，就可以知道第K大是誰了，遞迴查詢時，要注意一些值的代表意義。

合併兩棵Treap要用**啟發式合併**，也就是把小的合併進大的裡，這樣每次只要遍歷小的就好（這裡的合併跟Treap的合併不一樣，因為無法保證兩堆的Key值可以二分，所以必須遍歷其中一棵，一個一個點插入另外一棵），根據證明，這樣可以均攤一個點只會被合併 logN 次。

本來以為會有浮點數誤差，不過好像沒有~~。
**指標初始值**要記得給（NULL、0），不然可能會戳到奇怪的記憶體位置。
然後這體我不知道詢問有多少筆，先開個10^6 ~~。



``` c++
/* C++11 */
#include <cstdio>
#include <iostream>
#include <cstdlib>
#include <cstring>
#define F(n) Fi(i,n)
#define Fi(i,n) for(int i=0;i<n;i++)
#define N 20010
#define M 60010
#define K 1000005
#define LL long long
using namespace std;
struct Treap{
    Treap *l,*r;
    int key,pri,size;
    Treap(){}
    Treap(int _val):l(NULL),r(NULL),key(_val),pri(rand()),size(1){}
}*TT[N]={0};
struct MP{
    char c;
    int a,b;
    MP(){}
    MP(char _c,int _a,int _b):c(_c),a(_a),b(_b){}
}*ST[K]={0};
struct EE{
    int a,b;
    EE(){}
    EE(int _a,int _b):a(_a),b(_b){}
}*E[M]={0};
int F[N],st,tt,D[N];
bool wed[N],EL[M];
auto size = [](Treap *a){return a?a->size:0;};
auto pull = [](Treap *a){a->size = size(a->l)+size(a->r)+1;};
int go(int i,int now){
    return E[i]->a+E[i]->b-now;
}
int bos(int i){
    return F[i]==i?i:F[i]=bos(F[i]);
}
void split(Treap *t,int k,Treap *&a,Treap *&b){
    if(!t)a=b=NULL;
    else if(t->key <= k){
        a=t;
        split(t->r,k,a->r,b);
        pull(a);
    }else{
        b=t;
        split(t->l,k,a,b->l);
        pull(b);
    }
}
Treap* merge(Treap *a,Treap *b){
    if(!a||!b)return a?a:b;
    if(a->pri > b->pri){
        a->r=merge(a->r,b);
        pull(a);
        return a;
    }else{
        b->l=merge(a,b->l);
        pull(b);
        return b;
    }
}
void ins(Treap *&t,int k){
    Treap *tt=NULL;
    split(t,k,t,tt);
    t=merge(t,merge(new Treap(k),tt));
}
void del(Treap *&t,int k){
    Treap *lt=NULL,*rt=NULL,*tmp;
    split(t,k-1,lt,t);
    split(t,k,t,rt);
    tmp=t;
    t=merge(lt,merge(t->l,merge(t->r,rt)));
    delete tmp;
}
void DFS_merge(Treap *&a,Treap *&b){
    if(b->l)DFS_merge(a,b->l);
    if(b->r)DFS_merge(a,b->r);
    ins(a,b->key);
    delete b;
}
void merge_edge(int i){
    int ta=bos(E[i]->a),tb=bos(E[i]->b);
    if(ta!=tb){
        if(TT[ta]->size < TT[tb]->size){int tmp=ta;ta=tb;tb=tmp;}
        DFS_merge(TT[ta],TT[tb]);
        F[tb]=ta;
    }
}
int kth(Treap *t,int k){
    if(!t || k<=0 || k > t->size)return 0;
    if(size(t->r) == k-1)return t->key;
    if(size(t->r) > k-1)return kth(t->r,k);
    return kth(t->l,k-1-size(t->r));
}
int main(){
    int n,m,a,b,cas=1;
    char c;
    // srand(1020);
    while(scanf("%d%d",&n,&m),n||m){
        memset(E,0,sizeof(E));
        memset(ST,0,sizeof(ST));
        memset(EL,0,sizeof(EL));
        memset(wed,0,sizeof(wed));
        memset(TT,0,sizeof(TT));
        F(n)scanf("%d",D+i+1);
        F(m){
            scanf("%d%d",&a,&b);
            E[i+1]=new EE(a,b);
        }
        st=0;
        while(cin>>c){
            if(c=='E')break;
            if(c=='D')scanf("%d",&a),EL[a]=1;
            if(c=='C'){
                scanf("%d%d",&a,&b);
                int tmp=D[a];D[a]=b;b=tmp;
            }
            if(c=='Q')scanf("%d%d",&a,&b);
            ST[st++]=new MP(c,a,b);
        }
        F(n){
            F[i+1]=i+1;
            TT[i+1]=new Treap(D[i+1]);
        }
        F(m)if(!EL[i+1])merge_edge(i+1);
        LL sum=0,cnt=0;
        while(st--){
            if(ST[st]->c=='D'){
                merge_edge(ST[st]->a);
            }else if(ST[st]->c=='C'){
                del(TT[bos(ST[st]->a)],D[ST[st]->a]);
                D[ST[st]->a]=ST[st]->b;
                ins(TT[bos(ST[st]->a)],D[ST[st]->a]);
            }else{
                sum+=kth(TT[bos(ST[st]->a)],ST[st]->b);
                cnt+=1;
            }
            if(ST[st])delete ST[st];
        }
        double ans=sum*1.0/cnt;
        printf("Case %d: %lf\n",cas,ans);cas++;
        F(m)delete E[i+1];
    }
    return 0;
}
/*
5 4
10 20 20 20 20
1 2
2 3
3 4
4 5
Q 1 5
C 1 20
E
*/
```