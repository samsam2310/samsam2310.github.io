---
title: 'STEP5::Problem 0037 : Ch3-1.瀚瀚肥皂大戰比利'
tags:
  - C++
  - Step5
category:
  - Code 備忘錄
  - Greedy
date: 2014-03-26 05:43:28
---


http://web2.ck.tp.edu.tw/~step5/probdisp.php?pid=0037
感覺很有趣的題目。

<!--more-->

這題，個人感覺是，一開始如果已經大於等於了，那就是比利贏了；否則，能丟多少就丟多少，這時如果超過了，就是我贏了，但是要是還沒有超過，換比利時他應該也會丟九倍才對，但是如果是這樣我剛才就不應該丟九倍而是越少越好(我不應該自爆.....)，所以剛剛要改成兩倍，換比例丟九倍，所以/9*2*9--> *2，再來又換我， 既然沒死剛剛就應該丟九倍，而比利相反應該丟二(防止我用九倍贏他)，所以 /9*2/2*9-->*9，反覆跑到答案出來。至於其他倍數都一樣，要小最小要大最大，沒理由選他們。
以上個人的想法，不知道有沒有錯。



``` c++
#include <cstdlib>
#include <iostream>
#define LL long long
using namespace std;
int main(int argc,char *argv[])
{
    ios_base::sync_with_stdio(false);
    LL n;
    while(cin>>n){
        LL tmp=1;
        while(1){
            if(tmp>=n){cout<<"B_B\n";break;}
            tmp*=9;
            if(tmp>=n){cout<<"A_A\n";break;}
            tmp*=2;
        }
    }
    return 0;
}
```