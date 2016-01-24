---
title: 'STEP5::Problem 0098 : 刮鬍匹配'
tags:
  - C++
  - Step5
  - Stack
category:
  - Code 備忘錄
  - Data Structure
date: 2014-02-19 04:43:27
---


http://web2.ck.tp.edu.tw/~step5/probdisp.php?pid=0098
這題很簡單，其實先把0放到stack裡，如果放了1進去，就看他上一個是不是0，是就一起拔掉，不是就繼續堆上去，輸出。

<!--more-->



``` c++
#include <cstdio>
#include <cstdlib>
#include <iostream>
#define N 100001
using namespace std;
int ST[N],st=0;
int main(int argc,char *argv[])
{
    int n,b;
    scanf("%d",&n);
    for(int i=0;i<n;i++){
            scanf("%d",&b);
            if(b==1&&st>0){
                 if(ST[st-1]==0){st--;}
                 else ST[st++]=b;
            }else 
                  ST[st++]=b;
    }
    printf("%d\n",st);
    //test
    //system("pause");
    return 0;
} 
```