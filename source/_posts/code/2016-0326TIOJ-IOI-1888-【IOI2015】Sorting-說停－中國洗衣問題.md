---
title: 'TIOJ::IOI::1888 . 【IOI2015】Sorting 說停－中國洗衣問題'
tags:
  - C++
  - TIOJ
  - IOI
category:
  - Code 備忘錄
  - Else
date: 2016-03-26 08:36:38
---


http://tioj.ck.tp.edu.tw/problems/1888
這題是IOI 2015 Day2的題目，題目跟Day1的Scale都是要你排序，可是這題比較好寫XDD。

<!--more-->

題目給你一串1到N的序列，你必須透過交換兩個數字來排序，可是有一個人會來亂，那個人，估且叫他小明吧，他會依照一個自己的規則去交換兩個數字，而你會在他換完一次後，也跟著換一次，如果你換完後，序列已經成功排好序了，你們兩個就會停手，你希望找出最少需要幾次，還有你依序要怎麼交換兩個數字。

一開始你已經知道小明每一回合會交換哪兩個位置的數字了，我們先想一個叫作置換群的概念。其實也不是什麼很難的東西，就是如果你知道每個數字排好序後應該在哪個位置（這題中數字i應該在第i個位置），那你就去看看誰佔了你的位置，再看看誰佔了他的位置...，最後，你會繞一圈回來，這一個環就是一個置換群，如果環裡有k個數字，你很顯然可以用k-1次交換讓環裡所有數字歸位。
所以如果現在大小N的序列有j個環，那排序整個序列需要交換的次數就是N-j次。

有了以上性質，我們有一個離線作法，也就是二分搜答案，每次驗證一個答案L，先按照小明的交換順序去交換數字，自己則什麼都不做，到第L步時，檢查現在置換群的數量，如果小於等於L，代表存在一種作法可以在L步內排好序，因為你現在只要交換L次一定可以排好序，而且你只要在小明交換前先把你想換的數字換過來就好，小明只關心位置不關心裡面是什麼數字。複雜度O(N*logN)。

找出L之後，就摹擬找出交換策略。
先在第L步的狀態下，找出你要交換哪些數字。然後從一開始開始做，先讓小明交換一組數字，然後你從你需要交換的數字裡挑一組，看看他們現在在哪裡，交換這兩個位置，接著再讓小明交換第2組...依此類推，做完你就找到策略了。

關於一開始的二分搜，有一個很難寫的用Treap維護的在線找法，不過因為這題還要你找出策略，所以你還是必須離線找測略，但是你可以在線找出你需要幾次才能排好序。


``` c++
#include <bits/stdc++.h>
#define F(n) Fi(i,n)
#define Fi(i,n) Fl(i,0,n)
#define Fl(i,l,n) for(int i=l;i<n;i++)
using namespace std;
const int N = 200001;
typedef pair<int,int> PII;
PII CH[N*3];
int D[N], H[N];
bool wed[N];
void DFS(int now){
    wed[now] = true;
    if(!wed[D[now]])DFS(D[now]);
}
int check(int n){
    memset(wed,0,sizeof(wed));
    int res = 0;
    F(n)if(!wed[i])DFS(i),res++;
    return n - res;
}
main(){
    ios_base::sync_with_stdio(false);
    cin.tie(0);
    int t,n,m;
    cin>>t;
    while(t--){
        cin>>n;
        F(n)cin>>D[i],H[D[i]] = i;
        cin>>m;
        F(m)cin>>CH[i].first>>CH[i].second;
        int l = -1,r = n*3;
        while(l<r-1){
            int mid = (l+r)/2;
            F(n)D[H[i]] = i;
            F(mid)swap(D[CH[i].first], D[CH[i].second]);
            if(check(n) <= mid)r = mid;
            else l = mid;
        }
        cout<<r<<endl;
        memset(wed,0,sizeof(wed));
        vector<PII> ANS;
        F(n)D[H[i]] = i;
        F(r){
            swap(D[CH[i].first], D[CH[i].second]);
        }
        F(n)if(!wed[i]){
            int now = i;
            wed[now] = true;
            while(!wed[D[now]]){
                ANS.push_back(PII(D[now],D[D[now]]));
                now = D[now];
                wed[now] = true;
            }
        }
        if(ANS.size()<r)ANS.push_back(PII(0,0));
        assert(ANS.size() == r);
        F(n)D[H[i]] = i;
        F(r){
            int a = CH[i].first,b = CH[i].second;
            swap(H[D[a]],H[D[b]]);
            swap(D[a],D[b]);
            a = ANS[i].first, b = ANS[i].second;
            cout<<H[a]<<' '<<H[b]<<endl;
            swap(H[a], H[b]);
            swap(D[H[a]], D[H[b]]);
        }
    }
}
```