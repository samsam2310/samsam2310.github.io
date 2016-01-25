---
title: 'STEP5::Problem 0130 : 壓力好大'
tags:
  - C++
  - Step5
  - 背包問題
category:
  - Code 備忘錄
  - DP
date: 2014-02-21 05:41:33
---


http://web2.ck.tp.edu.tw/~step5/probdisp.php?pid=0130
這題是01背包問題的變形。先開一個寬度跟體力一樣陣列，每個元素初始成你銀行裡的錢，把任務依照賺的錢排序(得到-花費)，然後 DP[i]=MAX(DP[i],DP[i-體力]+得到-花費)。
注意DP順序要從後面跑到前面，不然會一個任務做兩次。最後輸出 DP[體力]那個。

<!--more-->



``` c++
#include <cstdio>
#include <cstdlib>
#include <iostream>
#define N 5001
using namespace std;

int ci[N],ai[N],pi[N],bapk[N];

void quickSort(int A[],int B[],int C[],int left, int right)
{
    int i, j, s , Temp;
    if(left < right) {
        s = A[(left+right)/2];
        i = left - 1;
        j = right + 1;
        while(1) {
            while(A[++i] < s) ;  // ¦V¥k§ä
            while(A[--j] > s) ;  // ¦V¥ª§ä
            if(i >= j) break;
               Temp = A[i];
               A[i] = A[j];
               A[j] = Temp;
               Temp = B[i];
               B[i] = B[j];
               B[j] = Temp;
               Temp = C[i];
               C[i] = C[j];
               C[j] = Temp;
        }
        quickSort(A,B,C, left, i-1);   // ¹ï¥ªÃä¶i¦æ»¼°j
        quickSort(A,B,C, j+1, right);  // ¹ï¥kÃä¶i¦æ»¼°j
    }
}

void work(int i,int p)
{
    if (p == 0)return;
    if (pi[i] > p)return;
    if (bapk[p- pi[i]] >= ci[i])
            bapk[p] = ( bapk[p]> bapk[p- pi[i]]+ ai[i])? bapk[p]: bapk[p- pi[i]]+ ai[i];
    work (i, p-1);
}

int main(int argc,char *argv[])
{
    int n,c,p;
    scanf ("%d %d %d", &n, &c, &p);
    for (int i=1; i<=n; i++){
            scanf ("%d %d %d", &ci[i], &ai[i], &pi[i]);
            if(ai[i]- ci[i]> 0) 
                     ai[i] -= ci[i];
            else {
                     i--;
                     n--;
            }
    }
    for (int i=1; i<=n; i++)
            bapk[i]= i;
    quickSort (ci, ai, pi, 1, n);
    for (int i=0; i<p; i++)
            bapk[i]= c;
    for (int i=1; i<=n; i++)
            work (i, p);
    printf ("%d\n", bapk[p-1]);
    return 0;
} 
```