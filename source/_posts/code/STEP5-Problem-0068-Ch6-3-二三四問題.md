---
title: 'STEP5::Problem 0068 : Ch6-3.二三四問題'
tags:
  - C++
  - Step5
  - Sort
category:
  - Code 備忘錄
  - String
date: 2014-02-20 05:33:12
---


http://web2.ck.tp.edu.tw/~step5/probdisp.php?pid=0068
單純字串處理，只是要注意數字尾巴直接接負號的情況(123-123)，為了方便我一次讀一行，抓完一次數字就把指標往回推一格。

<!--more-->

排序直接用 std::sort(array,array+x);  ，會從小排到大。也可以塞函示進去，頗方便。

``` c++
#include <cstdlib>
#include <cstdio>
#include <iostream>
#include <cstring>
#include <algorithm>
using namespace std;
int nu[20001];
char tt[10001];
bool nn(char a){
  return (a-'0'>=0&&a-'0'<=9);
}
int main(int argc,char *argv[])
{
  int t,st=0,ss=1;
  scanf("%d",&t);
  for(int i=0;i<t;i++){
    scanf("%s",tt);
    for(int j=0;j<strlen(tt);j++){
      if(tt[j]=='-'&&nn(tt[j+1])){
              j++;
              ss=-1;
      }
      if(nn(tt[j])){
              int tmp=0;
              while(nn(tt[j])){
                  tmp=tmp*10+tt[j]-'0';
                  j++;
              }
              nu[st++]=tmp*ss;
              j--;
              ss=1;
      }
    }
  }
  sort(nu,nu+st);
  printf("%d\n",st);
  for(int i=0;i<st;i++)printf("%d ",nu[i]);
  puts("");
  //system("pause");
  return 0;
}
```