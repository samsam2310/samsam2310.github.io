---
title: 'HOJ::Problem : 7 - 微波爐'
tags:
  - C++
  - HOJ
category:
  - Code 備忘錄
  - Math
date: 2014-03-27 05:47:01
---


http://hoj.twbbs.org.tw/judge/problem/view/7
觀察題目。

<!--more-->

其實是費氏數列，因為牽手的狀態是前兩個人的狀態(如果和上一個牽手)加上前一個人的狀態(如果不牽手)和。




``` c++
#include <cstdlib>
#include <iostream>
using namespace std;
int main(int argc,char *argv[])
{
    ios_base::sync_with_stdio(false);
    int n,tmp1=0,tmp2=1;
    cin>>n;
    for(int i=0;i<n;i++){
        tmp1=tmp1+tmp2;
        tmp1%=10;
        int ch=tmp1;tmp1=tmp2;tmp2=ch;
    }
    cout<<tmp2<<endl;
    return 0;
}
```