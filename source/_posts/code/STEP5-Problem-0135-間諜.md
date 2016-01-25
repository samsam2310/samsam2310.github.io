---
title: 'STEP5::Problem 0135 : 間諜'
tags:
  - C++
  - Step5
  - 拓撲排序
category:
  - Code 備忘錄
  - Graph
date: 2014-02-24 05:52:17
---


http://web2.ck.tp.edu.tw/~step5/probdisp.php?pid=0135
這題每個點出度都是1，指向任意一個點。

<!--more-->

開一個陣列存每個點的入度，另外一個存點A指向誰。
之後重複檢查入度為0的點，將他和他指向的點拔掉，並修改入度陣列。
最後可能會出現環。如果出現入度都不是0的情況(環)，就隨意拔掉一組，重新檢查0。
直到全部都變成0就結束了。



``` c++
#include <cstdio>
#include <cstdlib>
#include <iostream>
#define M 1000010
using namespace std;

void gogo(int,int*);

int wed[M],wa[M]={0},sum=0,n,p0,p1,p2;

int main(int argc,char* argv[])
{
    bool br;
    
    scanf("%d",&n);
    for(int i=1;i<n+1;i++){
           scanf("%d",&wa[i]);
           if(wed[wa[i]]>0)wed[wa[i]]+=1;
           else wed[wa[i]]=1;
           
    }
    p0=1;
    p2=p1=n+1;
    while(1){
             br=1;
             for(int i=p0;i<p2;i++){
                     //printf("ATA wed[i]:%d",wed[i]);
                     if(wed[i]==0){
                           gogo(i,&sum);
                           //puts("AA");
                           br=0;
                     }        
             }
             /*printf("wed:"); 
             for(int i=1;i<n+1;i++)printf("%d ",wed[i]);puts("");
             printf("wa:"); 
             for(int i=1;i<n+1;i++)printf("%d ",wa[i]);puts("\n");
             */
             if(br){
                     br=1;p1=p2;
                     while(wed[p0]==-1&&p0<n+2)p0++;
                     for(int i=p0;i<p1;i++){
                              if(wed[i]>0){gogo(i,&sum);br=0;break;}
                              if(wed[i]!=-1){br=0;p2=i+1;}
                     }
                     if(br)break;
             }
    
    }
    printf("%d\n",sum);
    //system("pause");
    return EXIT_SUCCESS;
}

void gogo(int nu,int *sum)//用來拔掉點。
{
     int go=wa[nu];
     if(wed[go]==-1){wed[nu]=-1;return;}
     wed[nu]=wed[go]=-1;
     *sum+=1;
     if(wed[wa[go]]!=-1)wed[wa[go]]-=1;
     if(wed[wa[go]]==0)gogo(wa[go],sum);
     return;
}
```