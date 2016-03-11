---
title: 'STEP5::Problem 0088 : 秀吉物質'
tags:
  - C++
  - Step5
  - Binary Search
category:
  - Code 備忘錄
  - Divide & Conquer
date: 2014-03-26 05:41:00
---


http://web2.ck.tp.edu.tw/~step5/probdisp.php?pid=0088
二分搜，分治驗證答案，終於會一些分治了。

<!--more-->

這題算是我第一次寫單純分治(之前有寫過樹分治但是感覺又不太一樣)，其實就是歸併排序(Merge sort)的變形。首先用二分搜找答案是多少，驗證答案 m 是否正卻得方式就是看看如果排序後， m 是不是第 k 個元素。 先算好前綴和，每次複製到另外一個陣列裡，遞迴做歸聘排序，return 之後會得到一個左右都排序好的數列，合併的同時枚舉左邊的元素和右邊元素的差(就是某段區間和)是否大於 m ，是就加一，因為兩邊都有排序，左邊的元素和右邊的元素可以持續向右跑不用每次都掃一遍，複雜度是 O(n)。
這份code是參考神學長的code，所以感覺效率很高，我自己現在應該寫不出來(指針指一指一定爛掉)........。
這題還有就是二分搜，每次我都是亂湊亂湊，有時就會TLE，關於這份code的二分搜，在平常保證會排掉戳過的答案，在最後，如果mid戳到答案(cn=k-1，cn是有幾個數比答案大)，會跑到else讓 r 變成答案減一，如果是答案的上一個(cn=k)，l 會跑到答案加二，r 還是指到答案減一，所以這樣可以找出答案。

其他有關二分搜可以參考我一個學長的說明：
http://momo-funnycodes.blogspot.tw/2012/05/tioj-1208-k.html

相關的題目
http://mikucode.blogspot.tw/2014/05/toj161.html



``` c++
#include <cstdlib>
#include <iostream>
#include <algorithm>
#define N 100001
using namespace std;
int n,k,S[N],M[N],T[N],cn;
void find_m(int l,int r,int m)
{
    if(l==r)return;
    int i=l,st=l;
    int p=l,mid=(l+r)/2;
    find_m(l,mid,m);
    find_m(mid+1,r,m);
    for(int j=mid+1;j<=r;j++){
        while(i<=mid&&T[i]<=T[j])S[st++]=T[i++];
        while(p<=mid&&T[j]-T[p]>m)p++;
        cn+=p-l;
        S[st++]=T[j];
    }
    while(i<=mid)S[st++]=T[i++];
    for(i=l;i<=r;i++)T[i]=S[i];
    //cout<<"DD "<<l<<' '<<r<<' '<<cn<<endl;
}
int main(int argc,char *argv[])
{
    ios_base::sync_with_stdio(false);
    cin>>n>>k;
    M[0]=S[0]=0;
    for(int i=1;i<=n;i++){cin>>S[i];M[i]+=S[i]+M[i-1];}
    int l=-200000000,r=200000001;
    while(l<=r){
        for(int i=0;i<=n;i++)T[i]=M[i];
        int mid=(l+r)/2;
        cn=0;
        find_m(0,n,mid);
        //cout<<l<<' '<<r<<" ("<<mid<<')'<<cn<<endl;
        if(cn>=k)l=mid+1;
        else r=mid-1;
    }
    cout<<r+1<<endl;
    // cn=0;
    // find_m(0,n,14);
    // cout<<cn<<endl;
    //system("pause");
    return 0;
}
```