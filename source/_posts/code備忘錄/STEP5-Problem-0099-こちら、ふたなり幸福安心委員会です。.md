---
title: 'STEP5::Problem 0099 : こちら、ふたなり幸福安心委員会です。'
date: '2014-02-25 06:45:54'
tags:
  - C++
  - Step5
  - RMQ
category:
  - Code 備忘錄
  - Data Structure
---


http://web2.ck.tp.edu.tw/~step5/probdisp.php?pid=0099
這題.....線段樹，尋找區間最小值。

<!--more-->

首先，根本神曲


{% youtube SuCFHxGJ9zQ %}


好..重點是，線段樹就是一顆完整2元數，我用遞迴的方式記錄他的每一格的參數，然後每一格紀錄該區間的最小值，這樣就能快速查詢區間最小值，又能快速更新區間最小值。 如果不更新，可以用O(1)查詢的 [sparse table](http://www.felix021.com/blog/read.php?1066)。 


``` c++
#include <cstdlib>
#include <iostream>
#include <special_0099.h>
#define N 1000001
using namespace std;
using namespace futa;
int gg[4*N],man[N];
int mklist(int le,int re,int id){
     if(le==re){
                gg[id]=man[le];
                return man[le];
     }
     int tmp=(re-le)/2,tmp1,tmp2;
     tmp1=mklist(le,le+tmp,id*2);
     tmp2=mklist(le+tmp+1,re,id*2+1);
     gg[id]=(tmp1<tmp2)? tmp1:tmp2;
     return gg[id];
}
int find(int a,int b,int le,int re,int id){
    if(re<a||le>b)return -1;
    if(a<=le&&re<=b)return gg[id];
    int tmp=(re-le)/2,tmp1,tmp2;
    if(le==re) return gg[id];
    tmp1=find(a,b,le,le+tmp,id*2);
    tmp2=find(a,b,le+tmp+1,re,id*2+1);
    if(tmp1!=-1 && tmp2!=-1)return (tmp1<tmp2)? tmp1:tmp2;
    else return (tmp1!=-1)? tmp1:tmp2;
}
int main(int argc,char *argv[]){
    int n=init(),*p=momo_gives_you_list_of_futa(),qq=momo_tells_you_q();
    for(int i=1;i<=n;i++)man[i]=*(p+i-1);
    mklist(1,n,1);
    pair<int,int> a;
    for(int i=0;i<qq;i++){
            a=momo_asks();
            you_tell_momo(find(a.first+1,a.second+1,1,n,1));
    }
    //system("pause");
    return 0;
} 
```