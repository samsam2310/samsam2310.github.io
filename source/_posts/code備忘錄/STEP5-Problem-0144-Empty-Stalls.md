---
title: 'STEP5::Problem 0144 : Empty Stalls'
tags:
  - C++
  - Step5
category:
  - Code 備忘錄
  - Else
date: 2014-02-20 05:37:37
---


http://web2.ck.tp.edu.tw/~step5/probdisp.php?pid=0144
這題的解題關鍵就是---把英文學好..........

<!--more-->

題目要把牛放進柵欄，因為一個柵欄只能放一個，所以準備一個推車，從1推到n，如果車上有牛就放一隻進去，如果柵欄裡有兩隻牛以上，就留一隻，其他裝上車。把柵欄開成兩倍大小，前後互補程式會比較好寫(吧?!)。



``` c++
#include <cstdio>
#include <cstdlib>
#include <iostream>
#define N 3000000
using namespace std;

int bar[2*N], n, k;

int main(int argc, char * argv[])
{
    scanf("%d %d", &n, &k);
    int x, y;
    long long a, b;
    for(int i = 0; i < k; i++){
            scanf("%d %d %lld %lld", &x, &y, &a, &b);
            for(int j = 1; j <= y; j++){
                    // printf("%d : %d %d %d %d",i,x,y,a,b);
                    bar[(a*j+b)%n] += x;
                    bar[(a*j+b)%n+n] = 1;
                    // printf("WW:%d\n", (a*j+b-1)%n+1);
            }
    }
    int car = 0;
    for(int i = 0; i < 2*n; i++){
            if(bar[i%n] >  1) {
                    car += bar[i%n]-1;
                    bar[i%n] = 1;
                    // printf("EE: %d\n", (i-1)%n+1);
            }
            if(bar[i%n] ==  0 && car > 0){
                    car --;
                    bar[i%n] = bar[i%n+n] = 1;
                    // printf("%d\n", (i-1)%n+1);
            }
    }

    // for (int i = 1; i <= 2*n; ++i)
    // {
    //      printf("%d ",bar[i]);
    // }

    for(int i = 0; i <= n; i++)
            if(bar[i] == 0){
                    printf("%d\n", i);
                    break;
            }
    return 0;
}
```