---
title: 'STEP5::Problem 0007 : Ch1-4.蘿莉的獎勵'
tags:
  - C++
  - Step5
  - Heap
category:
  - Code 備忘錄
  - Data Structure
date: 2014-02-27 08:35:35
---


http://web2.ck.tp.edu.tw/~step5/probdisp.php?pid=0007
水題~~
排序，從最大開始，-1-2-3..........直到等於0為止的前N項全部加起來輸出。
為了練習手co了heapㄏ.......

<!--more-->



``` c++
#include <cstdio>
#include <cstdlib>
#include <iostream>
#define N 100001
using namespace std;
int hp[N],hpa=1;
void hpup(int id)
{
     if(id==1)return;
     if(hp[id]<hp[id/2])return;
     int tmp=hp[id];hp[id]=hp[id/2];hp[id/2]=tmp;
     hpup(id/2);
     return;
}

void hpdw(int id)
{
     if(id==hpa-1)return;
     int pp;
     if(id*2+1<hpa)
             pp=(hp[id*2]>hp[id*2+1])? id*2:id*2+1;
     else if(id*2<hpa)
             pp=id*2;
     else return;
     
     if(hp[id]<hp[pp]){
             int tmp=hp[id];hp[id]=hp[pp];hp[pp]=tmp;
             hpdw(pp);
     }
}

void popo()
{
    for(int i=1;i<hpa;i++)
            printf("%d ",hp[i]);
    puts("");
}

void heap()
{
    for(int i=1;i<hpa;i++)
            hpup(i);
    return;
}

void hp_push(int a)
{
     hp[hpa++]=a;
     hpup(hpa-1);
}

void hp_pop()
{
     hpa--;
     hp[1]=hp[hpa];
     hpdw(1);
}

int main(int argc,char *argv[])
{
    int t,n;
    scanf("%d",&t);
    while(t--){
            hpa=1;
            scanf("%d",&n);
            for(int i=0;i<n;i++){
                    scanf("%d",&hp[hpa]);
                    hpa++;
            }
            heap();
            int sum=0;
            for(int i=1;hp[1]-i>0 && i<=n;i++){
                    sum+=hp[1]-i;
                    //printf("sum:%d hp[1]-i:%d\n",sum,hp[1]-i);
                    hp_pop();
            }
            printf("%d\n",sum);
    }
    //system("pause");
    return 0;
}
```