---
title: 'STEP5::Problem 0114 : 破：萌娘苦力怕'
tags:
  - C++
  - Step5
  - Hash
category:
  - Code 備忘錄
  - String
date: 2014-03-24 05:28:08
---


http://web2.ck.tp.edu.tw/~step5/probdisp.php?pid=0114
字串hash比對。

<!--more-->

這題要比對一個字串，其中的比對方式就是題目中說的，環狀相等。
第一就是字串比對時，可以用hash的方法O(1)比對(一般掃一遍是O(n) )，就是把字串當成一種27進位法，從右邊加一位就*27+字元，從左邊加字元就 字元*27的位數次方。因為數字很大，mod一個很大的數字，不過這樣有可能會有不一樣的字串一樣的數字的情況發生(我就遇到了.....)，這時可以換一個數字，或是多個數字去比對，越多錯誤率越小。

再來我就從第一個和最後一個開始比對，如果一樣就丟到steak裡，直到最大，再來從stack最上面的值開始繼續比第二段，最後最大值就是暫時的答案。然後，如果答案到n/2還有一段，就一段一段加入第二段，另一端把第一段退回stack裡的第二個位置(因為有可能這樣會有更優解)，重複執行直到stack空掉或是超過n/2。



``` c++
#include <cstdio>
#include <cstdlib>
#include <iostream>
#define N 1000010
#define Q2 1000007 //第9筆會WA
#define Q 7393913  //可以AC的質數
using namespace std;
char T[N];
int A,B,C,n,st[N/2],sta=0;
int main(int argc, char *argv[])
{
    scanf("%d",&n);
    scanf("%s",T);
    int m=0;
    int bn=(n%2)? n:n;
    int P[N/2]={1};
    for(int i=1;i<N/2;i++)P[i]=P[i-1]*27%Q;
    for(int i=0;i<bn/2;i++){
            A=A*27+T[i]-'a'+1;
            A%=Q;
            B+=(T[n-1-i]-'a'+1)*P[i];
            B%=Q;
            if(A==B){m=i+1;st[sta++]=m;}
            //printf("AA %d\n",A);
            //rintf("BB %d\n",B);
    }
    //if(m==0)system("pause");
    A=B=0;
    int m2=m;
    //for(int i=0;i<sta;i++)printf("%d ",st[i]);
    for(int i=m;i<bn/2;i++){
            A=A*27+T[i]-'a'+1;
            A%=Q;
            B+=(T[n-1-i]-'a'+1)*P[i-m2];
            B%=Q;
            if(A==B)m=i+1;
            //printf("AA %d\n",A);
            //printf("BB %d\n",B);
    }
    A=B=C=0;
    int m3=m;
    for(int i=m;i<bn/2&& m2-1>=0;i++){
            A=(A*27+T[i]-'a'+1)%Q;
            B=(B+(T[n-i-1]-'a'+1)*P[i-m3])%Q;
            C=(C+(T[--m2]-'a'+1)*P[i-m3])%Q;
            //printf("WW:%d %d %d\n",A,B,C);
            while(m2<st[sta-1])sta--;
            if(sta==0)break;
            if(m2==st[sta-1]){
                if(A==B&&B==C){
                    m=i+1;
                }
            }
    }
    printf("%d\n",m);
    //if(m==m3)system("pause");
    return 0;
}
```