---
title: 'TIOJ::1186 . 好多星星 ver1.1'
tags:
  - C++
  - TIOJ
category:
  - Code 備忘錄
  - Else
date: 2014-12-15 08:56:43
---


http://tioj.infor.org/problems/1186

有趣的小題目。

話說TIOJ網頁因為Markdown的關係，會把"輸出"的空白吃掉......。

<!--more-->



``` c++
#include <cstdio>
#define F(n) Fi(i,n)
#define Fi(i,n) for(int i=0;i<n;i++)
main(){
    int n;
    while(scanf("%d",&n),n){
        F(n-1){
            Fi(j,i)putchar(' ');putchar('*');
            Fi(k,2){
                Fi(j,n-2-i)putchar(' ');
                putchar('*');
            }
            putchar('\n');
        }
        F(n*2-1)putchar('*');putchar('\n');
        F(n-1){
            Fi(j,n-2-i)putchar(' ');putchar('*');
            Fi(k,2){
                Fi(j,i)putchar(' ');
                putchar('*');
            }
            putchar('\n');
        }
    }
}
```