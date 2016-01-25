---
title: 'TOJ::GCD'
tags:
  - C++
  - TOJ
category:
  - Code 備忘錄
  - Math
date: 2014-02-25 05:58:10
---


就...最大公因數，輾轉相除法，遞迴寫法。

<!--more-->



``` c++
#include <cstdio>
#include <cstdlib>
#include <iostream>
using namespace std;
int gcd(int a,int b)
{
    if(b==0)return a;
    else gcd(b,a%b);
}
int main(int argc, char *argv[])
{
    int t,a,b;
    scanf("%d",&t);
    while(t--){
               scanf("%d %d",&a,&b);
               printf("%d\n",gcd(a,b));
    }
    return 0;
}
```