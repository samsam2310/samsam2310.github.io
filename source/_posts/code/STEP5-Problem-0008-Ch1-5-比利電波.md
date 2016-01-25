---
title: 'STEP5::Problem 0008 : Ch1-5.比利電波'
tags:
  - C++
  - Step5
  - Tree
  - Binary Search
category:
  - Code 備忘錄
  - Graph
date: 2014-03-06 04:34:46
---


http://web2.ck.tp.edu.tw/~step5/probdisp.php?pid=0008
這題題目是給你平衡二元樹上的節點，有些會往子節點傳遞電波，有些會阻擋電波。問總共有多少節點會有電波。

<!--more-->

直觀的想法是建二元樹去跑，但是範圍太大，所以利用二元樹的節點有 n ，2n， 2n+1，的關係這點，我們可以先算出每一層的範圍，推算出每一點在哪一層，將點座標除2再二分搜他的父節點或是祖先(至少會有根節點)，如果性質不同(會不會傳導)，就加或減掉該節點的子節點數(會傳導就加，不會就扣掉)；如果性質相同，因為會重複計算所以就忽略該節點。
全部連完之後答案就出來了。



``` c++
#include <cstdio>
#include <cstdlib>
#include <iostream>
#include <algorithm>
#define N 100001
#define LL long long
using namespace std;
int t, n, h;
LL H[64];
struct MP{
       LL a;
       int b;
} zb[N];
inline bool operator<(const MP &a, const MP &b)
{
       return (a.a > b.a)? true : false;
}
int main(int argc, char *argv[])
{
    H[0] = 1;
    for(int i = 1; i < 64; i++)
            H[i] = H[i-1] * 2;
    scanf("%d",&t);
    while(t--){
               scanf("%d %d",&h, &n);
               for(int i = 0; i < n; i++)
                       scanf("%d %d", &zb[i].a, &zb[i].b);
               zb[n] = (MP){1,1};
               sort(zb,zb+n);
               //cout <<"AA"<<sum<<endl;
               LL sum = 0;
               int hi = 1;
               for(int i = 0; i < n; i++){
                       LL p = i+1, ta = zb[i].a /2,l=p,r=n;
                       while(ta != zb[p].a){
                                //cout <<"AA\n";
                                while(ta > zb[p].a)ta/=2;
                                l=i+1;r=n;
                                while(l!=r){
                                            //printf("RR %lld %lld\n",l,r);
                                            p=(l+r)/2;
                                            if(zb[p].a > ta)l=p+1;
                                            else if(zb[p].a < ta)r=p;
                                            else break;
                                            p=(l+r)/2;
                                }
                                //printf("EE %lld\n",p);
                                //system("pause");
                       }
                       //printf("%lld -> %lld\n",zb[i].a,zb[p].a);
                       hi=1;
                       if(zb[i].b != zb[p].b){
                                  while(zb[i].a <= H[h-hi]-1) hi++;
                                  if(zb[i].b) sum += H[hi]-1;
                                  else  sum += -H[hi]+1;
                       }
               }
               sum += H[h]-1;
               printf("%lld\n", sum);
    }
    //system("pause");
    return 0;
} 
```