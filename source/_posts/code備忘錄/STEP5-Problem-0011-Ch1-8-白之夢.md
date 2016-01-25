---
title: 'STEP5::Problem 0011 : Ch1-8.白之夢'
tags:
  - C++
  - Step5
category:
  - Code 備忘錄
  - Math
date: 2014-02-27 08:38:28
---



http://web2.ck.tp.edu.tw/~step5/probdisp.php?pid=0011
機率吧.....要找最大值，但是只能走一次，所以要猜他是不是最大的。

<!--more-->

先跑 0.37~0.27 的N(好像是機率....就是這樣)，紀錄裡面最大的(tmp)，然後跑完剩下的N，如果比tmp大就選他，結束。如果沒有就選最後一個，不然會WA。



``` c++
#include <cstdlib>
#include "special_0011.h"
#include <iostream>
using namespace std;
int main(int argc,char *argv[])
{
    int t=Start_The_Loli_Dream();
    while(t--){
            int n=Count_How_Many_Loli();
            int m=0;
            for(int i=0;i<n*3/8;i++){
                    int tmp=Get_Loli_Moeness();
                    m=(m>tmp)? m:tmp;
            }
            for(int i=n*3/8;i<n;i++){
                    int tmp=Get_Loli_Moeness();
                    if(tmp>m){
                        You_Choose_This_Loli();
                        goto end;
                    }
            }
            You_Choose_This_Loli();
            end:;
    }
    return 0;
}
```