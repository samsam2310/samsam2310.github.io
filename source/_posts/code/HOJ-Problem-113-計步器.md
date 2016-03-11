---
title: 'HOJ::Problem : 113 - 計步器'
tags:
  - C++
  - HOJ
  - Tree
category:
  - Code 備忘錄
  - Divide & Conquer
date: 2014-03-24 05:29:14
---


http://hoj.twbbs.org.tw/judge/problem/view/113
樹分治。

<!--more-->

樹分治，本來覺得很難，實作一遍之後感覺還可以........。
基本上要先找樹重心(也就是一個點當根的話，所有子樹中點最多的子樹點最少的情況)，這是用來優化樹分治的，不然最悲劇的情況會回到O(N^2)。
我一開始重心找爛了.....，樹重心是指子樹中點最多的數量和剩下的點(就是父節點以上的點)一樣多(或差不多一樣)的狀況。(我寫成子樹點的總和......)
這題的重點，先從一點開始(最好是重心)，對每顆子樹做DFS，DFS時計算到根的距離%m，並算出這個數字減m的值是多少，去set裡用lower_bound找最接近的(加上他剩下%m才會最大)，更新最大值，要注意第一顆子樹DFS時set裡沒東西，所以要先丟一個0進去。做完一顆子樹後，將剛剛那些距離全丟到set裡，繼續第二..第三......顆子樹，全做完後，把set丟掉，把函式遞迴處理所有的子樹，對子樹做一模一樣的事(小心不要跑回這次的根)，全做完後，就得到答案了。
一開始TLE，所以我捨棄STD::set改用vector，每次都push到vector裡，但是lower_bound時只找原本傳下去時的範圍，要找下一顆子樹時，更改範圍並排序，這樣可以節省set跟vector合併的時間，而且set insert要O( log n)，這樣結果比較快。



``` c++
#include <cstdlib>
#include <iostream>
#include <algorithm>
#include <vector>
#include <set>
#define N 100001
#define LL int
using namespace std;
int t,n,m,ch[N],hr,mi;
bool wed[N];
int find_h(vector<LL> *D,LL now,LL no,LL al){
    ch[now]=1;
    int mx=0;
    for(int i=0;i*2<D[now].size();i++){
        if(D[now][i*2]!=no&&!wed[D[now][i*2]]){
            int tmp=find_h(D,D[now][i*2],now,al);
            ch[now]+=tmp;
            mx=tmp>mx? tmp:mx;
        }
    }
    if(abs(n-ch[now]-mx)<mi){hr=now;mi=abs(n-ch[now]-mx);}
    return ch[now];
}
int DFS(vector<int> *D,vector<int> *S,int lo,int now,int no,int l){
    S->push_back(l);
    LL mx=(*(--lower_bound(++S->begin(),S->begin()+lo,m-l)))+l;
    //cout<<now<<' '<<mx<<endl;
    for(int i=0;i*2<D[now].size();i++){
        LL tmp=0;
        if(D[now][i*2]!=no&&!wed[D[now][i*2]])tmp=DFS(D,S,lo,D[now][i*2],now,(D[now][i*2+1]+l)%m);
        mx=(mx>tmp)? mx:tmp;
    }
    return mx;
}
int tre(vector<int> *D,int now,int no){
    vector<int>S;
    vector<int>chc;
    S.push_back(0);
    S.push_back(0);
    S.push_back(0);
    //cout<<'!'<<' '<<(*(--lower_bound(++S.begin(),S.end(),0)))<<endl;;
    int lo=S.size();
    LL mx=0,tmp=0;
    for(int i=0;i*2<D[now].size();i++){
        hr=D[now][i*2];
        if(D[now][i*2]!=no&&!wed[D[now][i*2]])tmp=DFS(D,&S,lo,D[now][i*2],now,D[now][i*2+1]%m);
        mx=(mx>tmp)? mx:tmp;
        chc.push_back(S.size()-lo);
        sort(S.begin(),S.end());
        lo=S.size();
    }
    S.clear();
    wed[now]=true;
    int st=0;
    for(int i=0;i*2<D[now].size();i++){
        if(D[now][i*2]!=no&&!wed[D[now][i*2]]){
            mi=2147483647;
            //find_h(D,D[now][i*2],now,chc[st++]);
            tmp=tre(D,D[now][i*2],0);
            //cout<<now<<' '<<tmp<<endl;
        }
        mx=(mx>tmp)? mx:tmp;
    }
    return mx;
}
int main(int argc,char * argv[])
{
    ios_base::sync_with_stdio(false);
    cin>>t;
    srand(10248431);
    while(t--){
        fill(wed,wed+N,0);
        vector<LL>D[N];
        cin>>n>>m;
        for(int i=0;i<n-1;i++){
            LL x,y,z;
            cin>>x>>y>>z;
            D[x].push_back(y);
            D[x].push_back(z%m);
            D[y].push_back(x);
            D[y].push_back(z%m);
        }
        mi=2147483647;
        find_h(D,1,0,n);
        cout<<tre(D,hr,0)<<endl;
    }
    return 0;
}
```