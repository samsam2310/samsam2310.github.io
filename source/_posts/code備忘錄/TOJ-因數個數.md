---
title: 'TOJ::因數個數'
tags:
  - C++
  - TOJ
category:
  - Code 備忘錄
  - Math
date: 2014-02-25 05:59:50
---


先建根號N的質數表，然後再質因數分解求解，質因數分解時，只要是除到質數J大於根號A就可以了１，因為這樣A一定是質數，再輸出解的時候再多*2就好，如果是完全平方數或是某數的冪次，那A就會剩下1，這時直接輸出答案。

<!--more-->



``` c++
#include <cstdio>
#include <cstdlib>
#include <iostream>
#define N 46400
using namespace std;
int ST[N],st=0;
bool S[N]={0};
int main(int argc,char *argv[])
{
    int t,a;
    S[0]=S[1]=1;
    for(int i=2;i<N;i++){
            if(!S[i]){
                      ST[st++]=i;
                      if(i<220)for(int j=i*i;j<N;j+=i)S[j]=1;
            }
    }
    scanf("%d",&t);
    while(t--){
               scanf("%d",&a);
               int sum=1;
               if(a<=N)
                      if(!S[a]){
                                puts("2");
                                continue;
                      }
               for(int i=0;ST[i]*ST[i]<=a&&i<st;i++){
                       int tmp=1;
                       while(a%ST[i]==0){tmp++;a/=ST[i];}
                       sum*=tmp;
                       if(a==1)break;
               }
               if(a==1)printf("%d\n",sum);
               else printf("%d\n",sum*2);
    }
    return 0;
}
```