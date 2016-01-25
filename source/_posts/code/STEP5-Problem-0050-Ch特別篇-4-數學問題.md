---
title: 'STEP5::Problem 0050 : Ch特別篇-4.數學問題'
tags:
  - C++
  - Step5
category:
  - Code 備忘錄
  - Math
date: 2014-03-04 04:00:50
---


http://web2.ck.tp.edu.tw/~step5/probdisp.php?pid=0050
真的是水題....。

<!--more-->

題目要求的所有弓形面積，其實會拼成很多個圓，只要多一條線，就多一個圓。所以若點的數量是 n，線的數量就是 n*(n-1)/2，就等於圓的數量，題目會給圓的半徑和點的數量，直接輸出答案就好，點的座標可以直接忽略......。



``` c++
#include <cstdlib>
#include <iostream>
#include <algorithm>
#include <cmath>
#include <iomanip>
#define pi 3.14159265359
using namespace std;
int main(int argc,char *argv[])
{
    double r,n;
    ios_base::sync_with_stdio(false);
    cout <<fixed<< setprecision(6);
    cin>>n;
    n=n*(n-1)/2;
    cin>>r;
    r=pow(r,2);
    cout<<r*n*pi<<endl;
    // system("pause");
    return 0;
}
```