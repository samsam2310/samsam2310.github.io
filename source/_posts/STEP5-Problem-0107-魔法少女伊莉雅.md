---
title: 'STEP5::Problem 0107 : 魔法少女伊莉雅'
tags:
  - C++
  - Online Judge
category:
  - Code 備忘錄
  - Math
date: 2014-02-19 04:57:01
---


http://web2.ck.tp.edu.tw/~step5/probdisp.php?pid=0107
這題算是水題。我早期用C的code好醜........
注意r=0的情況，然後可以直接用 `printf("%.6f\n",float);`  輸出，他會自動4捨5入，很方便~

<!--more-->



``` c++
#include <stdio.h>
#include <stdlib.h>
int t,n,k,l,n1;
float a,r,all;
int main(int argc, char *argv[])
{
  scanf("%d",&t);
  for(k=0;k<t;k++){
    scanf("%d",&n);
    all=0;
    n1=n;
    for(l=0;l<n1;l++){
        scanf("%f %f",&r,&a);
        if(r==0||a==0)
            n-=1;
        else
            all+=a/(r*r);
    }
    all=all/n;
    printf("%.6f\n",all);
  }
  return 0;
}
```