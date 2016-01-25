---
title: 'UVa::10929 - You can say 11'
tags:
  - C++
  - UVa
category:
  - Code 備忘錄
  - Else
date: 2014-03-27 05:51:04
---


http://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=24&page=show_problem&problem=1870
檢查一個大數是不是11的倍數。

每次吃一位數，然後原本的總和乘10加上這位數，在mod 11，最後等於0，就是11的倍數，反之則不是。注意測資開頭有可能是0(像是 0005, 0023845)。

<!--more-->



``` c++
#include <cstdlib>
#include <iostream>
#include <cstring>
#define N 1001
#define F(n) Fi(i,n)
#define Fi(i,n) Fl(i,0,n)
#define Fl(i,l,n) for(int i=l;i<n;i++)
using namespace std;
int main(int argc,char *argv[])
{
    ios_base::sync_with_stdio(false);
    char NU[N];
    while(cin>>NU){
        int n=strlen(NU),st=0;
        if(NU[0]=='0'&&n==1)break;
        F(n){
            st=st*10+NU[i]-'0';
            st%=11;
        }
        if(st)cout<<NU<<" is not a multiple of 11.\n";
        else cout<<NU<<" is a multiple of 11.\n";
    }
    return 0;
}
```