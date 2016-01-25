---
title: 'STEP5::Problem 0103 : Transcend'
tags:
  - C++
  - Step5
category:
  - Code 備忘錄
  - Greedy
date: 2014-03-24 05:30:38
---


http://web2.ck.tp.edu.tw/~step5/probdisp.php?pid=0103
推出規則後.......水水的題目。

<!--more-->

算Greedy，從0開始，每次選後面最小的去看，然後把區間縮小，直到最小值變成n(我先把他設為最大值，加到n-1後面)，就回頭一次，區間回到全部，直到全部都看過(看過的數量等於n)

因為範圍很小很小，for迴圈就過了，我一開始還寫線段樹.....。



``` c++
#include <cstdlib>
#include <iostream>
#define MX 2147483647
#define N 1001
using namespace std;
int S[N];
bool wed[N];
int main(int argc,char * argv[])
{
    int n;
    cin>>n;
    for(int i=0;i<n;i++)cin>>S[i];
    int st=0,ans=1,mi=0;
    S[n]=MX;
    while(st!=n){
        for(int i=mi+1;i<=n;i++){
            if(!wed[i]){
                mi=S[mi]>=S[i]? i:mi;
            }
        }
        if(mi==n){
            ans++;
            mi=0;
        }else{
            st++;
            S[mi]=MX;
        }
    }
    cout<<ans<<endl;
}
```