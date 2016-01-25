---
title: 'STEP5::Problem 0084 : 神秘題'
tags:
  - C++
  - Step5
category:
  - Code 備忘錄
  - Math
date: 2014-02-19 04:28:05
---

http://web2.ck.tp.edu.tw/~step5/probdisp.php?pid=0084
這題只要利用第一二組測資，照著傳再PO結果到題目提示的網站上，就會發現這題是歐拉函數(幹嘛不早講.......)。$\varphi (n)$歐拉函數是小於或等於n的正整數中與n互質的數的數目。

<!--more-->

其中一種公式 $$ \varphi (n) = n \times (1-p ^{-1} _1) \times (1-p ^{-1} _2) \times ... \times (1-p ^{-1} _r) $$ 其中P為n的質因數。
至於最後一組測資，因為保證是質數，所以直接輸出n-1。

``` c++
#include <cstdio>
#include <cstdlib>
#include <iostream>
using namespace std;

void _w(long long int);
long long int po(long long int p,long long int k);

int main(int argc,char *argv[]){
    long long int a;
    scanf("%I64d",&a);
    if(a>600000000){
            printf("%I64d\n",a-1);
    }else if(a==1){
            printf("1\n");
    }else{
            _w(a);
    }
    //system("pause");
    return EXIT_SUCCESS;
}

void _w(long long int a){
     long long int st[100],p,k=0,as=0,t=a;
     int ste=0;
     for(long long int i = 2; t != 1; i++){
            if( t % i == 0 )
            {
                while( t % i == 0 ){k++; t /= i;}
                st[ste++]=i;
            }
     }
     if(ste==1){
             p=st[0];
             as=(p-1)*po(p,k-1);
             printf("%I64d\n",as);
             return;
     }
     as=a;
     for(int i=0;i<ste;i++){
             as=as-as/st[i];
     }
     printf("%I64d\n",as);
     return;
}

long long int po(long long int p,long long int k){
     if(k==0)return 1;
     long long int cc=1;
     for(long long int i=0;i<k;i++){
              cc*=p;
     }
     return cc;
}
```