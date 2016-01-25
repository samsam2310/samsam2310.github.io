---
title: 'STEP5::Problem 0101 : 夢幻的恆定狀態'
tags:
  - C++
  - Step5
  - Deque
category:
  - Code 備忘錄
  - DP
date: 2014-03-06 04:49:41
---


http://web2.ck.tp.edu.tw/~step5/probdisp.php?pid=0101
這題題目要我們把1到N加起來......，但是沒這麼簡單。

這題題目有點......，他說要彈琵琶，但是手太寬會彈到旁邊的弦.......，對於每根弦給你他的快樂度，你會從1彈到N，其中因為手太大，可能會彈到很多條，那就取其中快樂度最高的。
首先，設兩個指標P1、P2，如果手的下緣搆不到P1指的弦，就往右移；如果手搆得到P2指的弦，P2也往右，然後找出P1到P2-1之間的最大值，我用deque實作，再來就從1跑到N就好了。

<!--more-->

所謂的deque，就是一個可以先進先出或先進後出，結合queue和stack的資料結構，我們可以透過維持他的單調性來將複雜度降到O(n)。
首先，將值push進去，如果進去前的那個值比他小，代表就算有最大值也不會是他了，因為我們的雙指針只會往右，所以他已經沒利用價值了，就用stack的方法pop掉。重復直到deque呈現一個非嚴格遞減序列，最大值永遠是第一個。當有值"過期"時，如果和第一個一樣，就代表他過期了，就用queue的方法pop掉，每個值只會進出deque一次，故複雜度為O(n)。



``` c++
#include <cstdlib>
#include <iostream>
#include <queue>
#define N 1000001
#define LL long long
#define F(n) Fi(i,n)
#define Fi(i,n) Fl(i,0,n)
#define Fl(i,l,n) for(int i=l;i<n;i++)
using namespace std;
int S[N];
LL X[N];
int main(int argc,char *argv[])
{
    ios_base::sync_with_stdio(false);
    int k;
    LL w,sum=0;
    cin>>k>>w;
    F(k)cin>>S[i];
    F(k)cin>>X[i];
    deque<int> dd;
    dd.push_back(S[0]);
    int l=0,r=1;
    F(k){
        while(X[l]<X[i]-w){
            if(dd.front()==S[l])dd.pop_front();
            l++;
        }
        while(X[r]<=X[i]+w){
            while(!dd.empty()&&dd.back()<S[r])dd.pop_back();
            dd.push_back(S[r++]);
        }
        sum+=dd.front();
        //cout<<dd.front()<<endl;
    }
    cout<<sum<<endl;
    cin>>k;
    return 0;
}
```