---
title: 'STEP5::Problem 0006 : Ch1-3.陷阱'
tags:
  - C++
  - Step5
category:
  - Code 備忘錄
  - Else
date: 2014-02-26 08:32:58
---


http://web2.ck.tp.edu.tw/~step5/probdisp.php?pid=0006
不知道分哪類......。
題目是要讓1~任意點的區間和大於0，每次可以讓一個點+1或是將最後的點移到前面，問最少步數。

<!--more-->

CODE真的好醜，應該有更好的寫法......
我的方法是，先將全部加起來(p)，並記錄最小的區間和是多少(q)，這個點離尾端多遠(S)，然後判斷，如果p小於0就看要加多少才會到1，這些是一定要花費的步數(不然去哪裡搬更大的數字??)，再來看看修好的陷阱壓不壓得過q(如果負太多修了也沒用)，壓的過就直接輸出了，但如果壓不過就表示好的陷阱都在S那段，那就要判斷把S這段搬過來跟修好q差多少，也就是要修陷阱還是搬陷阱會比較快，(把S全搬到前面一定可以壓過q)。



``` c++
#include <stdio.h>
#include <stdlib.h>
int t,n,sm,u[1000000];
long long int p,q,s,s2,smin;
int main(int argc, char *argv[])
{
  scanf("%d\n",&t);
  for(int i=0;i<t;i++){
          p=0;q=1;
          scanf("%d\n",&n);
          for(int l=0;l<n;l++){
                 scanf("%d",&u[l]);
                 p+=u[l];
                 //printf("DD:%I64d\n",p);
                 if(p<q){q=p;sm=n-l;}
          }
          if(p>0) s=0;
          else    s=0-p+1;
          //printf("UU:%I64d\n\n",u[n-1]);
          if(q<=0&&p>q){
                 if(p>0){
                         s2=0;
                         smin=-q+1;
                         for(int l=1;s2<=-q;l++){
                                 s2+=u[n-l];
                                 s+=1;
                                 if(-q-s2+1>=0){ if(s-q-s2+1<smin)smin=s-q-s2+1;
                                 }else{          if(s<smin)smin=s;}
                                 //printf("SS:%I64d\n",smin); 
                         }
                         s=smin;
                 }else if(u[n-1]+s>-q)  s+=1;
                  else{
                         smin=s-q-u[n-1]+p;
                         //printf("EE:%I64d %d\n",smin,sm);
                         s2=0;
                         for(int l=1;l<sm && l<-q-u[n-1]+p;l++){
                                 s2+=u[n-l];
                                 s+=1;
                                 if(-q-s2+p-1>=0){ if(s-q-s2+p-1<smin)smin=s-q-s2+p-1;
                                 }else{  if(s<smin)smin=s;  }
                                 //printf("FF:%I64d %I64d\n",smin,s2);   
                         }
                         s=smin;
                 }
          }
          //printf("%I64d\n",s);
  }
  //system("PAUSE");    
  return 0;
}
```