---
title: 'STEP5::Problem 0105 : 內地歌手ジャン'
tags:
  - C++
  - Step5
  - Sort
category:
  - Code 備忘錄
  - Else
date: 2014-05-23 07:32:39
---

http://web2.ck.tp.edu.tw/~step5/probdisp.php?pid=0105
就是排序。題目怪怪的，應該是小到大...。

<!--more-->



``` c++
#include <cstdlib>
#include <iostream>
#include <algorithm>
using namespace std;
#define N 500001
#define F(n) Fi(i,n)
#define Fi(i,n) Fl(i,0,n)
#define Fl(i,l,n) for(int i=l;i<n;i++)
int S[N];
int main(int argc,char *argv[])
{
    ios_base::sync_with_stdio(false);
    int n;
    cin>>n;
    F(n)cin>>S[i];
    sort(S,S+n);
    F(n)cout<<S[i]<<' ';cout<<endl;
    return 0;
}
```