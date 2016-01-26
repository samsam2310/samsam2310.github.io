---
title: 'UVa::1153 - Keep the Customer Satisfied'
tags:
  - C++
  - UVa
  - Heap
category:
  - Code 備忘錄
  - Greedy
date: 2015-10-21 09:45:08
---


https://uva.onlinejudge.org/external/11/p1153.pdf
之前入營考出現過的題目。
有點會讓人誤會的Greedy。

<!--more-->

題目說我們有一堆訂單，每個訂單都有一個工作時間和一個期限，問我們最多可以接幾筆訂單。

題目給了兩個提示，
第一個是按照期限做，
第二個是，
先按照期限先後做，如果要做一個工作V的時候發現做不完（時間超過），那就看看之前做的工作，如果有一個工作U花的時間比現在這個工作久，那就把那個工作U換成V。

提示二會對的原因，是因為我們可以發現，同樣做不完一個工作，做工作V和U都是做一個工作，但是U反而花更久時間，換成V的話我還可以剩下更多時間。

實作的話只要排個序然後開個Heap。



``` c++
#include <bits/stdc++.h>
#define F(n) Fi(i,n)
#define Fi(i,n) Fl(i,0,n)
#define Fl(i,l,n) for(int i=l;i<n;i++)
using namespace std;
const int N = 800001;
typedef pair<int,int> PII;
PII D[N];
main(){
    ios_base::sync_with_stdio(false);
    cin.tie(0);
    int t,n,sum;
    cin>>t;
    while(t--){
        cin>>n;
        F(n)cin>>D[i].first>>D[i].second;
        sort(D,D+n,[](const PII&a,const PII&b){return a.second<b.second;});
        priority_queue<PII>qq;
        sum=0;
        F(n){
            if(sum+D[i].first>D[i].second&&!qq.empty()&&qq.top().first>D[i].first){
                sum -= qq.top().first;
                qq.pop();
            }
            if(sum+D[i].first<=D[i].second){
                sum += D[i].first;
                qq.push(D[i]);
            }
        }
        cout<<qq.size()<<endl;
        if(t)cout<<endl;
    }
    // while(1);
}
```