---
title: 'STEP5::Problem 0129 : 驗算'
date: '2014-02-25 07:27:46'
tags:
  - C++
  - Step5
  - Monte Carlo
category:
  - Code 備忘錄
  - Math
---


http://web2.ck.tp.edu.tw/~step5/probdisp.php?pid=0129
這題是矩陣乘法，但是直接乘一定會爆掉(n^3)，所以要利用矩陣乘法的結合率(矩陣沒有交換率，但有結合率)，先randen一個1*n的矩陣L，(A*L)*(B*L)=(C*L)，這樣的情況有兩種:


<!--more-->


1.不一樣，錯了就錯了
2.對了，不一定對(因為這樣算法會出現剛好一樣得狀況)

這就延伸出雖機的算法了，隨機演算法有 [Las Vegas algorithm](https://en.wikipedia.org/wiki/Las_Vegas_algorithm) 和 [Monte Carlo algorithm](https://en.wikipedia.org/wiki/Monte_Carlo_algorithm)等等，其中 Las Vegas是一種猜測答案會對的演算法，而Monte Carlo則是利用機率來提高自己答案的正確性，以這題來說，因為錯了就錯了，但是對了有1/2的機會還是錯的，所以就多randen幾個L，做五次錯的機率就變成 1/2^5=1/32，基本上就AC了，而且出測資的人又不知道你的randen是多少ㄏ....。



``` c++
#include <cstdlib>
#include <iostream>
#include <cstdio>

using namespace std;

void pa(int[][1000],int[],int[]);
void mkl(int[]);
int in(void);

int n,a[1000][1000],b[1000][1000],c[1000][1000];
char t[1];
int main(int argc, char *argv[])
{
    
    srand(2);
    //scanf("%d\n",&n);
    n=in();
    
    //gets(t);
    for(int i=0;i<n;i++){
            for(int l=0;l<n;l++){
                    a[i][l]=in();//scanf("%d",&);
            }
    }
    
    for(int i=0;i<n;i++){
            for(int l=0;l<n;l++){
                    b[i][l]=in();//scanf("%d",&);
            }
    }
    //printf("%d %d",c[0][0],c[0][1]);
    
    //puts("YYY");
    for(int i=0;i<n;i++){
            for(int l=0;l<n;l++){
                    c[i][l]=in();//scanf("%d",&);
            }
    }
    
    for(int h=0;h<5;h++){        
            int l[1000]={0},lb[1000]={0},lab[1000]={0},lc[1000]={0};
            mkl(l);
            pa(b,l,lb);
            pa(a,lb,lab);
            pa(c,l,lc);
            
            for(int i=0;i<n;i++){
                   if(lab[i]!=lc[i]){
                           //printf("%d %d  ",lab[i],lc[i]);
                           printf("Bad\n");
                           //system("PAUSE");
                           return EXIT_SUCCESS;
                   }
            }
            
            
    }        
    printf("Good\n");
    //else{
            /*scanf("%d",&w);
            if(n>950&&w<100){
                   printf("Good\n");
            }else if(w>1000){
                   printf("Good\n");
            }else{
                   printf("Bad\n");
            }*/
            
    //}
    
    //system("PAUSE");
    return EXIT_SUCCESS;
}

void pa(int aa[][1000],int ll[],int lf[]){
       for(int i=0;i<n;i++){
              for(int l=0;l<n;l++){
                      lf[i]+=ll[l]*aa[i][l];
              }
              //printf("%d  ",lf[i]);
       }
}

void mkl(int _l[]){
       //_l[0]=1;
      //_l[1]=2;
       
       
       for(int i=0;i<n;i++){
               _l[i]=rand()%10;
       }
}

int in()   
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
     if(cha==' '||cha=='\n') break;   
      x=x*10+cha-48;   
    }   
    return x*flag;   
}
```