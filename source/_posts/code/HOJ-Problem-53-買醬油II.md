---
title: 'HOJ::Problem : 53 - 買醬油II'
tags:
  - C++
  - HOJ
category:
  - Code 備忘錄
  - DP
date: 2014-03-13 04:57:18
---


http://hoj.twbbs.org.tw/judge/problem/view/53
買醬油系列之2，卡特蘭數。

<!--more-->

只是單純的卡特蘭數，只是注意不能直接用排列組合的C階乘公式，因為mod運算不支援除法運算，會爛掉.....
用遞迴式先預處理10000項在O(1)查表。



``` c++
#include <cstdlib>
#include <iostream>
#define LL long long
#define Q 910193
#define N 10001
using namespace std;
LL S[N];
int main(int argc,char *argv[])
{
    ios_base::sync_with_stdio(false);
    int t,l;
    S[0]=1;
    for(int i=1;i<N;i++){
            for(int j=0;j<i;j++){
                    S[i]+=S[j]*S[i-j-1];
                    S[i]%=Q;
            }
    }
    cin>>t;
    while(t--){
            cin>>l;
            cout<<S[l]<<endl;
    }
    return 0;
}
```