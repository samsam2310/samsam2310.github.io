---
title: 'HOJ::Problem : 13 - 矩形周長'
tags:
  - C++
  - HOJ
  - POJ
  - 線段樹
  - 掃描線
category:
  - Code 備忘錄
  - Data Structure
date: 2014-05-24 07:39:09
---


http://poj.org/problem?id=1177
http://hoj.twbbs.org.tw/judge/problem/view/13
這題是[矩形覆蓋面積](/code/HOJ-Problem-12-矩形面積覆蓋/)的延伸問題，矩形覆蓋周長。


基本上是覆蓋面積的變形，但還是有一些難理解....。

<!--more-->

同樣有兩個重點。

首先要改變線段樹的結構，增加一個整數，兩個布林值，分別代表現在的邊數和左界右界是否有邊。
邊分成平行掃描線和垂直掃描線，線段樹處理的是垂直的。
講得明白一點，一個矩形可以拆成兩個矩形拼在一起，線段樹切下去就會變成好多塊矩形拼在一起，當線段樹回傳時，若左邊的右界有邊，右邊的左界有邊，那麼其實這兩塊區間是接在一起的，所以實際的邊數要減二。

所以如果區間被完全覆蓋，邊數等於2。如果不完全或沒覆蓋，邊數等於左邊節點邊數+右邊節點邊數 ，但如果他們中間有接再一起要扣掉兩條。
至於左界右界是否有邊的布林值，如果區間完全覆蓋則都是TRUE，否則就等於左子節點的左和右子節點的右。要注意如果左界==右界就直接設為FALSE。

這樣合併上去可以得到垂直邊數，乘上掃描的長度就是邊長，再來就是平行邊。

平行邊的長度等於每次更新後線段覆蓋的長度差的絕對值，也就是更新前減更新後的絕對值，要注意的是，如果有X座標一樣的邊，一定要先做矩形左邊的邊再做矩形右邊的邊，推導一次就可以知道，要是反過來會多算到一段，要是一次做完同X座標的邊會少算到一段。

PS.還有另外一種做法是掃兩遍，一次X一次Y軸，每次都只處理平行邊，這樣就不用改線段樹。只是比較笨。



``` c++
#include <cstdlib>
#include <iostream>
#include <cstring>
#include <algorithm>
#define M 20001
#define N 5001
#define F(n) Fi(i,n)
#define Fi(i,n) Fl(i,0,n)
#define Fl(i,l,n) for(int i=l;i<n;i++)
#define LL long long
using namespace std;
struct MP{
    int x,t,d,tag;
}D[N*2];
inline bool operator<(MP a,MP b){
    if(a.x==b.x)return a.tag>b.tag;
    return a.x<b.x;
}
struct tp{
    int x,tag,ln;
    bool l,r;
}BST[4*M];
int get_ln(int id){
    return BST[id].tag? 2:BST[id].ln;
}
int query(int l,int r,int a,int b,int id,int x){
    if(l>b||r<a)return BST[id].tag? r-l+1:BST[id].x;
    if(l==r){
        BST[id].tag+=x;
        BST[id].l=BST[id].r=BST[id].tag>0;
        BST[id].ln=BST[id].tag? 2:0;
        return BST[id].tag? 1:0;
    }
    if(a<=l&&r<=b){
        BST[id].tag+=x;
        if(BST[id].tag>0){
            BST[id].l=BST[id].r=true;
            return r-l+1;
        }else{
            BST[id].l=BST[id*2].l;
            BST[id].r=BST[id*2+1].r;
            return BST[id].x;
        }
    }
    BST[id].x=query(l,(l+r)/2,a,b,id*2,x)+query((l+r)/2+1,r,a,b,id*2+1,x);
    BST[id].ln=get_ln(id*2)+get_ln(id*2+1)-2*(int)(BST[id*2].r&&BST[id*2+1].l);
    BST[id].l=BST[id].tag? true:BST[id*2].l;
    BST[id].r=BST[id].tag? true:BST[id*2+1].r;
    return BST[id].tag? r-l+1:BST[id].x;
}
int main(int argc,char *argv[])
{
    ios_base::sync_with_stdio(false);
    int n;
    cin>>n;
    F(n){
        cin>>D[i*2].x>>D[i*2].t>>D[i*2+1].x>>D[i*2].d;
        D[i*2+1].t=D[i*2].t;
        D[i*2+1].d=D[i*2].d;
        D[i*2].tag=-(D[i*2+1].tag=-1);
    }
    sort(D,D+n*2);
    LL sum=0;
    int cu=0,ncu=0,now=-10000;
    F(n*2){
        sum+=(LL)((D[i].x-now)*get_ln(1));
        now=D[i].x;
        ncu=query(0,M-1,D[i].t+10000,D[i].d+9999,1,D[i].tag);
        sum+=(LL)((cu-ncu)>0? cu-ncu:ncu-cu);
        cu=ncu;
        //cout<<"DD "<<i<<' '<<sum<<' '<<cu<<endl;
    }
    cout<<sum<<endl;
    //cin>>n;
    return 0;
}
```