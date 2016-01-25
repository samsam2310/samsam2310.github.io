---
title: 'STEP5::Problem 0087 : 中♂位♂數'
tags:
  - C++
  - Step5
category:
  - Code 備忘錄
  - Math
date: 2014-02-25 06:00:03
---


http://web2.ck.tp.edu.tw/~step5/probdisp.php?pid=0087
這題題目會讓你判斷一個亂序數列的中位數(你只知道第幾個但不知道數字)，每次可以卻定A、B、C(任意三數)誰是這三數中的中位數。

<!--more-->

做法是先固定一個AB，然後枚舉C，如果B是中位數把C塞到"右邊"陣列，如果C是中位數塞到中間，如果A是中位數塞到左邊。
做完後可以切成5段，左、A、中、B、右，看看數量就可以知道中位數在哪一段，如果不是A或B就用A或B重複執行上述步驟到那段只剩下1個數字，就得到中位數了。
要注意重複執行的時候A或B要固定，才能確保左邊、右邊不會塞反了(反了就悲劇...)
code很醜很想重寫......



``` c++
#include <cstdio>
#include <cstdlib>
#include <iostream>
#include "special_0087.h"
using namespace std;

//int W[10]={0,7,3,5,4,2,6,9,1,8};


int _mid(int[],int[],int[],int,int,int);
int Med3(int,int,int);
void chh(int[],int);

int n,M,le=0,ri=0,TT[1500];

int main(int argc,char *argv[])
{
    srand(2);
    int as,a,b,T1[1500],T2[1500],T3[1500],t1=0,t2=0,t3=0;
    n=GetN();
    //n=9;
    M=(n+1)/2; 
    for(int i=0;i<n;i++)
            T1[i]=i+1;
    t1=n-2;
    a=n-1;
    b=n;
    
    //printf("%d ",T1[n-1]);
    as=_mid(T1,T2,T3,a,b,t1);
    
    //Answer(as);
    printf("%d\n",as);
    system("pause");
    return EXIT_SUCCESS;
}



int _mid(int T1[],int T2[],int T3[],int a,int b,int tt){
    int l,t1=0,t2=0,t3=0;
    
    for(int i=0;i<tt;i++)TT[i]=T1[i];
    //for(int i=0;i<tt;i++)printf("%d ",TT[i]);
    //puts("\n");
    
    for(int i=0;i<tt;i++){
           l=Med3(a,b,TT[i]);
           if(l==a){
                     T3[t3++]=TT[i];
           }else if(l==b){
                     T1[t1++]=TT[i];
           }else if(l==TT[i]){
                     T2[t2++]=TT[i];
           }
    } 
    
    
    /*for(int i=0;i<t1;i++)printf("%d(%d) ",T1[i],W[T1[i]]);
    puts(" T1\n");
    for(int i=0;i<t2;i++)printf("%d(%d) ",T2[i],W[T2[i]]);
    puts(" T2\n");
    for(int i=0;i<t3;i++)printf("%d(%d) ",T3[i],W[T3[i]]);
    puts(" T3\n");*/
    
    //printf("1:%d 2:%d\n",le,ri);
    
    if(le+t1+1==M){
            //puts("L\n");
            return b;
    }else if(ri+t3+1==M){
            //puts("R\n");
            return a;
    }else if(le+t1+1>M){
            //puts("L-\n");
            if(t1==1)return T1[0];
            ri+=t2+t3+1;
            chh(T1,t1);
            return _mid(T1,T2,T3,b,T1[t1-1],t1-1);
    }else if(ri+t3+1>M){
            //puts("R+\n");
            if(t3==1)return T3[0];
            le+=t1+t2+1;
            chh(T3,t3);
            return _mid(T3,T1,T2,T3[t3-1],a,t3-1);
    }else{
            //puts("LR\n");
            if(t2==1)return T2[0];
            le=le+t1;
            ri=ri+t3+1;
            chh(T2,t2);
            return _mid(T2,T1,T3,T2[t2-1],b,t2-1);
    }
    
}

void chh(int TT[],int tt)
{
     int pi=rand()%(tt-1),tmp=TT[pi];
     TT[pi]=TT[tt-1];TT[tt-1]=tmp;
     return;
}

/*
int Med3(int A,int B,int C){
    int a=W[A];
    int b=W[B];
    int c=W[C];
    
    //printf("A:%d B:%d C:%d\n",a,b,c);
    
    if(a>b&&a<c)return A;
    if(a<b&&a>c)return A;
    if(a>b&&b>c)return B;
    if(a<b&&b<c)return B;
    if(a>c&&b<c)return C;
    if(a<c&&b>c)return C;
}

*/

/*
int GetN()¡G¤@¶}©l´N©I¥s¡A¥B¤£§t¥ô¦ó°Ñ¼Æ¡F¦^¶ÇNªº­È¡C 
2.int Med3(int x,int y,int z)¡G©I¥s®É»Ýµ¹3­Ó¤£¦P¤Hªº¥N¸¹·í°Ñ¼Æ¡F¦^¶Ç³o3­Ó¤H¤¤¹ê¤O©~©ó¤¤¶¡ªº¤H¥N¸¹¡C 
3.void Answer(int ans)*/
```