---
title: 'STEP5::Problem 0004 : Ch1-1.一切的開始'
tags:
  - C++
  - Step5
category:
  - Code 備忘錄
  - DP
date: 2014-02-26 08:31:43
---


http://web2.ck.tp.edu.tw/~step5/probdisp.php?pid=0004
感覺有點DP的一題。要把所有I換成J，最少要幾步。

<!--more-->

題目中，可以對字串做兩種操作，
1.把I換J，J換成I
2.把1~A個全部做第一件事，算一個動作。
把他想成01，目標全部變成1，我用a0,a1紀錄到目前為止要全部變成0、1要花幾步的時間，最後輸出少的那個(a0的話要再加一，因為要先全部變成1)
有一個重點，就是遇到1，a0++；遇到0，a1++，但是a0,a1不會差超過1步，因為如果變成0(或1)需要比變成1(或0)多兩步，那直接從另外一個整排變過來就好。



``` c++
#include <cstdio>
#include <cstdlib>
#include <iostream>
using namespace std;

int n,a0=0,a1=0;
char a;

int main(int argc,char *argv[])
{
    scanf("%d",&n);
    while(a=getchar())if(a=='0'||a=='1')break;
    if(a=='1')a0=1;
    else a1=1;
    for(int i=1;i<n;i++){
            a=getchar();
            if(a=='1'&&a0<=a1)a0++;
            else if(a=='0'&&a1<=a0)a1++;
            
            printf("0:%d 1:%d\n",a0,a1);
    }
    a0++;
    //printf("%d\n",(a1<=a0)? a1:a0);
    //system("pause");
    return EXIT_SUCCESS;
}
```