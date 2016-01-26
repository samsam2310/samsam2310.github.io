---
title: 'HOJ::Problem : 12 - 矩形面積覆蓋'
tags:
  - C++
  - HOJ
  - 線段樹
  - 掃描線
category:
  - Code 備忘錄
  - Data Structure
date: 2014-05-24 07:37:21
---


http://hoj.twbbs.org.tw/judge/problem/view/12
矩形覆蓋面積的經典題，需要思考一下，還頗有趣的。
他的類似題還有矩形周長。

<!--more-->

這題想了很久，看了題解後，又想了一段時間才有想法。

基本上，這題分成兩個重點，掃描線跟線段樹。

掃描線的部分，我們把矩形拆成左邊的線跟右邊的線，然後用一條掃描線掃過去，碰到左邊就加上去，碰到右邊就扣掉，就能得到每次往右移動時總共有多少面積被覆蓋，將這個數字乘上向右移動的距離，就是掃過的面積。
可能有點抽像，有附上圖片的說明。

線段樹則是用來維護覆蓋面積，這裡的線段樹有點特化，我想了好久想不出來。
重點是，因為永遠是全域查詢，所以只要確定最後的回傳值是對的就可以了，我用了一個struct當做線段樹的單位。
當更新的範圍覆蓋住左界跟右界時，把tag加上更新值。
完全沒有覆蓋到就直接回傳。
不完全覆蓋時，遞迴下去並把回傳值的和更新到這格的數字。

回傳時，如果tag大於零，代表這個區間被完全覆蓋，回傳右界減左界加一。如果tag等於0，代表區間沒有被覆蓋或是不完全覆蓋，回傳這格記錄的數字。

之後只要把矩形拆開，照X座標排序，依序更新並往右掃描，至於相同X座標不用額外處理，因為位移是0，不影響總和。



``` c++
#include <cstdlib>
#include <iostream>
#include <cstring>
#include <algorithm>
#define M 30001
#define N 10001
#define F(n) Fi(i,n)
#define Fi(i,n) Fl(i,0,n)
#define Fl(i,l,n) for(int i=l;i<n;i++)
using namespace std;
struct MP{
    int x,t,d,tag;
}D[N*2];
inline bool operator<(MP a,MP b){
    return a.x<b.x;
}
struct tp{
    int x,tag;
}BST[4*M];
int query(int l,int r,int a,int b,int id,int x){
    //if(x==0)cout<<l<<' '<<r<<' '<<BST[id].tag<<endl;
    if(l>b||r<a)return BST[id].tag? r-l+1:BST[id].x;
    if(a<=l&&r<=b)return (BST[id].tag+=x)? r-l+1:BST[id].x;
    BST[id].x=query(l,(l+r)/2,a,b,id*2,x)+query((l+r)/2+1,r,a,b,id*2+1,x);
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
    int sum=0,cu=0,now=0;
    F(n*2){
        sum+=(D[i].x-now)*cu;
        now=D[i].x;
        cu=query(0,M-1,D[i].t,D[i].d-1,1,D[i].tag);
        //cout<<"DD "<<i<<' '<<sum<<' '<<cu<<endl;
    }
    cout<<sum<<endl;
    return 0;
}
```