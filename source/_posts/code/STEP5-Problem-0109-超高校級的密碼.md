---
title: 'STEP5::Problem 0109 : 超高校級的密碼'
tags:
  - C++
  - Step5
category:
  - Code 備忘錄
  - Math
date: 2014-02-19 05:10:04
---


http://web2.ck.tp.edu.tw/~step5/probdisp.php?pid=0109
這題算是mod運算吧，將文字轉成數字後，在開d 、e次方，然後mod29。

<!--more-->

因為d、e很大，一定會爆long long，還好有那個.....費馬小定理?!
$$a^{{p-1}}\equiv 1{\pmod  {p}}$$所以先把d、e都mod28就好了。
至於最後那個函式是快速輸入(對這題沒啥用，但是可以加快速度，宣告時前面應該要加inline)
code都是if海......



``` c++
#include <cstdlib>
#include <iostream>
#include <cstdio>
#include <cstring>
long long int in(void); 
using namespace std;
long long int e,d,p,q,ke,x,x2,lo;
char a[1025];
int main(int argc, char *argv[])
{
    e=in();
    d=in();
    q=in();
    for(int k=0;k<q;k++){
            p=in();
            if(p==1){
                     ke=e%28;
            }else{
                     ke=d%28;
            }
            gets(a);
   /* for(l=0;l<1025;l++){
      scanf("%s",b);
      if((strlen(b))==0){l=1024;}
      strcat(a," ");
      strcat(a,b);
    }
    */
            lo=strlen(a);
            for(int l=0;l<lo;l++){
                  if(a[l]==' ')
                        x=x2=0;
                  else if(a[l]==',')
                        x=x2=27;
                  else if(a[l]=='.')
                        x=x2=28;
                  else
                        x=x2=a[l]-'a'+1;
                  for(int w=0;w<ke-1;w++){
                        x*=x2;
                        x=x%29;
                       // printf("%d %d\n",w,x);
                  }
                  if(x==0)
                            a[l]=' ';
                  else if(x==27)
                            a[l]=',';
                  else if(x==28)
                            a[l]='.';
                  else
                        a[l]=x+'a'-1;
                  nono:
                  printf("%c",a[l]);
            }
            printf("\n");
  /*  for(l=0;l<1025;l++){
      printf("%c",a[l]);
      if(a[l]=='\0'){
      printf("\n");
      l=1024;
      }
    }*/
  }
  //system("pause");
  return 0;
}
long long int in()   //快速輸入
{   
  char cha;   
  long long int x=0,flag=1;   
  while(cha=getchar())   
  if(cha!=' '&&cha!='\n')break;   
  if(cha!='-'){
       x=x*10+cha-'0';  
   }else{
         flag=-1; 
   }  
  while(cha=getchar())    
    {   
     if(cha==' '||cha=='\n') break;   
      x=x*10+cha-48;   
    }   
    return x*flag;   
}
```