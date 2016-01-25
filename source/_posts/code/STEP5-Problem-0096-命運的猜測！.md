---
title: 'STEP5::Problem 0096 : 命運的猜測！'
tags:
  - C++
  - Step5
  - Binary Search
category:
  - Code 備忘錄
  - Else
date: 2014-02-21 05:44:22
---

http://web2.ck.tp.edu.tw/~step5/probdisp.php?pid=0096
互動題。
很簡單的2分搜~~


<!--more-->



``` c++
#include <cstdlib>
#include "special_0096.h"
#include <iostream>
using namespace std;
int main(int argc,char *argv[])
{
    int a=1,b=getn();
    while(1){
        if(suisoku((a+b)/2))b=(a+b)/2;
        else a=(a+b)/2;
    }
    return 0;
}
```