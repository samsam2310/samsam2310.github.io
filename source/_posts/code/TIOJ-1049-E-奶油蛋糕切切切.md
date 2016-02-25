---
title: TIOJ::1049 . E.奶油蛋糕切切切
tags:
  - C++
  - TIOJ
  - NPSC
category:
  - Code 備忘錄
  - Math
date: 2016-02-14 19:51:30
---

http://tioj.ck.tp.edu.tw/problems/1049

數學幾何題，有很多浮點數很麻煩，要想仔細一點。

<!--more-->

題目給你一個正方形，還有一堆可以分割這個正方形的線段，問最後正方形會被這些線段分成幾個平面。

很容易發現，每加一刀，會多出幾個平面，和這一刀跟之前的每條線的交點有關係。
每多一個交點，就會多一個平面，可是如果有兩個交點重疊了，那就會少一個平面。
所以先把所有交點排序，如果前一個和這個交點沒有重疊，平面數就加一。

要注意和浮點數有關的所有運算都要考慮誤差，在判斷等於的時候，如果兩個值得差的絕對值小於$10^{-9}$的話，就當作他們是等於的。


``` c++
#include <bits/stdc++.h>
#define F(n) Fi(i,n)
#define Fi(i,n) Fl(i,0,n)
#define Fl(i,l,n) for(int i=l;i<n;i++)
using namespace std;
const int N = 101;
const double EXP = 1e-9; 
typedef pair<double,double> PDD;
inline bool deq(double a,double b){
    return abs(a-b) < EXP;
}
struct line{
    PDD p,v;
    line(){}
    line(PDD a, PDD b):p(a),v(b.first-a.first,b.second-a.second){}
    PDD intersect(line b){
        if(deq(v.first,0)){
            if(deq(b.v.first, 0))return PDD(-1000,-1000);
            else{
                double s = (p.first-b.p.first)/b.v.first;
                return PDD(b.p.first+b.v.first*s, b.p.second+b.v.second*s);
            }
        }else if(deq(v.second,0)){
            if(deq(b.v.second, 0))return PDD(-1000,-1000);
            else{
                double s = (p.second-b.p.second)/b.v.second;
                return PDD(b.p.first+b.v.first*s, b.p.second+b.v.second*s);
            }
        }else{
            double mul = -v.second/v.first;
            double tmp = b.v.first*mul+b.v.second;
            if(deq(tmp, 0))return PDD(-1000,-1000);
            else{
                double s = ((p.first - b.p.first)*mul+(p.second - b.p.second))/tmp;
                return PDD(b.p.first+b.v.first*s, b.p.second+b.v.second*s);
            }
        }
    }
}D[N];
inline bool in_sqr(PDD a){
    return !deq(a.first,0)&&!deq(a.first,1)&&a.first>0&&a.first<1&&
            !deq(a.second,0)&&!deq(a.second,1)&&a.second>0&&a.second<1;
}
inline bool pteq(PDD a, PDD b){
    return deq(a.first,b.first) && deq(a.second, b.second);
}
PDD ST[N];
int st;
main(){
    ios_base::sync_with_stdio(false);
    cin.tie(0);
    int n, ans;
    PDD a,b;
    while(cin>>n, n){
        ans = 1;
        F(n){
            cin>>a.first>>a.second>>b.first>>b.second;
            D[i] = line(a,b);
            st = 0;
            Fi(j,i){
                ST[st] = D[j].intersect(D[i]);
                if(in_sqr(ST[st]))st++;
            }
            ST[st++] = a;
            ST[st++] = b;
            sort(ST,ST+st);
            Fi(j,st-1){
                // cout<<"DD "<<ST[j].first<<' '<<ST[j].second<<endl;
                if(!pteq(ST[j],ST[j+1]))ans++;
            }
            // cout<<"END DD"<<endl;
        }
        cout<<ans<<endl;
    }
}
```