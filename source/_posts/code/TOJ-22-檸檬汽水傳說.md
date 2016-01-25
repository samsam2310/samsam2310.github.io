---
title: 'TOJ::22 / 檸檬汽水傳說'
tags:
  - C++
  - TOJ
  - NPSC
  - Stack
category:
  - Code 備忘錄
  - Data Structure
date: 2014-02-26 08:25:53
---


這題是NPSC初賽題，題目要問有哪幾個區間中間的所有數字都小於或等於兩邊。

<!--more-->

我用for迴圈跑n^2，紀錄3個數字，f,s,tmp，f是左邊的點，s是右邊的點，tmp是中間最高的點(有可能是s)，如果fs就看s有沒有大於tmp，有就sum++，f==s就sum++。
然後tmp=s或是tmp(大的那個)，接下來s往後推一格。
不過跑ZERO OJ會TLE......

### 2016/1/25
**應該要做stack優化**，可是我那時候不知道XDD
其實就是丟進stack裡面，這篇就當作爆搜版吧XD。



``` c++
#include <cstdio>
#include <cstdlib>
#include <iostream>
#define N 1000001
using namespace std;
int S[N];
int main(int argc,char *argv[])
{
    int t,n;
    scanf("%d",&t);
    while(t--){
        scanf("%d",&n);
        for(int i=0;i<n;i++)
            scanf("%d",&S[i]);
        int sum=0;
        for(int i=0;i<n-1;i++){
            int f=S[i],s=S[i+1],tmp=S[i+1];
            for(int j=i+1;j<n;j++){
                //printf("%d(f:%d s:%d tmp:%d =%d)\n",i,f,s,tmp,sum);
                if(f<s){sum++;break;}
                else if(f==s)sum++;
                else if(s>=tmp)sum++;
                //printf("EE:%d\n",sum);
                tmp=(tmp>s)? tmp:s;
                s=S[j+1];
            }
        }
        printf("%d\n",sum);
    }
    return 0;
}
```