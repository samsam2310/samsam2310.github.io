---
title: 'STEP5::Problem 0012 : Ch2-1.言靈'
tags:
  - C++
  - Step5
category:
  - Code 備忘錄
  - Math
date: 2014-02-25 08:16:08
---


http://web2.ck.tp.edu.tw/~step5/probdisp.php?pid=0012
這題求n個數字中任意數字加起來為m的倍數。

<!--more-->

因為題目有說 N>=M，所以--N個數字中亂亂加一定會出現M的倍數。
所以只要求區間合等於M的倍數就好。
一開始輸入先MODm，等於0就輸出。
不等於零就加到sum裡，sum持續MODm，如果等於0就輸出1到i。
如果不等於，就在S[sum]中記錄i是多少，下一次如果sum又等於這次的sum(S[sum]不等於0)，就代表中間這段和等於0(m的倍數)，就輸出這段。
輸入優化因為有測資結尾不是 空白或換行，害我一開始TLE.....

### 2016/1/25
真看不懂我以前在寫甚麼鬼，題目有保證一定有解，跟N>=M沒有關係...。

``` c++
#include <cstdio>
#include <cstdlib>
#include <iostream>
#define N 5000001
#define LL long long
using namespace std;
int S[N]={0};
inline int in()//輸入優化
{   
  char cha;   
  unsigned int x=0,flag=1;
  while(cha=getchar())   
  if(cha!=' '&&cha!='\n')break;
  if(cha!='-'){
       x=x*10+cha-'0';  
   }else{
         flag=-1; 
   }  
  while(cha=getchar())    
    {   
     if(cha<'0'||cha>'9')break;
      x=x*10+cha-48;   
    }   
    return x*flag;
}
int main(int argc,char *argv[])
{
    //while(int aa=in())printf("%d\n",aa);
    int n,m,a;
    //scanf("%d %d",&n,&m);
    n=in();m=in();
    LL sum=0,tmp=0;
    for(int i=1;i<=n;i++){
            //scanf("%d",&a);
            a=in();
            tmp=(a%m+m)%m;
            if(!tmp){printf("1\n%d\n",i);goto end;}
            sum=(sum+tmp)%m;
            //printf("DD:%d(%d)\n",i,sum);
            if(sum==0){
                printf("%d\n1",i);
                for(int j=2;j<=i;j++)
                        printf(" %d",j);
                puts("");
                goto end;
            }
            else if(S[sum]){
                //printf("%d(%d)\n",i-S[sum],i);
                printf("%d\n%d",i-S[sum],S[sum]+1);
                for(int j=S[sum]+2;j<=i;j++)
                        printf(" %d",j);
                puts("");
                goto end;
            }else
                S[sum]=i;
    }
    
    end:;//system("pause");
    return 0;
}
```