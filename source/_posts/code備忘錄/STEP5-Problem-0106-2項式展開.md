---
title: 'STEP5::Problem 0106 : 2項式展開'
tags:
  - C++
  - Step5
category:
  - Code 備忘錄
  - Math
date: 2014-02-19 04:55:06
---


http://web2.ck.tp.edu.tw/~step5/probdisp.php?pid=0106
這題基本上是巴斯卡三角形，但是邊算邊跑一定TLE，所以先開一個陣列存起來。

<!--more-->



``` c++
#include <stdio.h>
#include <stdlib.h>
 
 
long long a[70][70];
int n,k,l;
  
int main(int argc, char *argv[])
{
  a[0][0]=1;
  a[1][0]=a[1][1]=1;
  for(k=2;k<70;k++){
    a[k][0]=a[k][k]=1;
    for(l=1;l<k;l++){
      a[k][l]=a[k-1][l-1]+a[k-1][l];
    }
  }
  /*
  while(scanf("%d", &n) != EOF){
  printf("%llu %d",a[n][n/2],a[n][n/2]);
  }
  */
    
  while(scanf("%d", &n) != EOF){
  //n+=2;
  printf("1");
  if(n==0){printf("\n");continue;}
  if(n==1){printf("+x\n");continue;}
  printf("+%llux",a[n][1]);
  if(n>2){
    for(l=2;l<n;l++){
      printf("+%llux^%d",a[n][l],l);
    }
     
  }
  printf("+x^%d",n);
  printf("\n");
    
    
  }
  return 0;
}
code hosted by snipt.net
#include <stdio.h>
#include <stdlib.h>
 
 
long long a[70][70];
int n,k,l;
  
int main(int argc, char *argv[])
{
  a[0][0]=1;
  a[1][0]=a[1][1]=1;
  for(k=2;k<70;k++){
    a[k][0]=a[k][k]=1;
    for(l=1;l<k;l++){
      a[k][l]=a[k-1][l-1]+a[k-1][l];
    }
  }
  /*
  while(scanf("%d", &n) != EOF){
  printf("%llu %d",a[n][n/2],a[n][n/2]);
  }
  */
    
  while(scanf("%d", &n) != EOF){
  //n+=2;
  printf("1");
  if(n==0){printf("\n");continue;}
  if(n==1){printf("+x\n");continue;}
  printf("+%llux",a[n][1]);
  if(n>2){
    for(l=2;l<n;l++){
      printf("+%llux^%d",a[n][l],l);
    }
     
  }
  printf("+x^%d",n);
  printf("\n");
    
    
  }
  return 0;
}
```