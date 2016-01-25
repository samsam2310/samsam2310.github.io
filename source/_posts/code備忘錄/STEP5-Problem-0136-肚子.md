---
title: 'STEP5::Problem 0136 : 肚子'
tags:
  - C++
  - Step5
category:
  - Code 備忘錄
  - Graph
date: 2014-02-24 05:55:26
---


http://web2.ck.tp.edu.tw/~step5/probdisp.php?pid=0136
這題因為每個器官之間都有一條單向腸道。

<!--more-->

所以如果出度最多，那那個器官只要兩步就會走到所有器官了(仔細模擬一次就會發現)
首先建表直接紀錄A指向B或B指向A，然後紀錄出度，我建了一顆線段樹去統計最大值，搜尋就輸出，轉向就更新。



``` c++
#include <cstdio>
#include <cstdlib>
#include <iostream>
#define N 2000

int n, q, p[N], BIT[4*N];//其實不是BIT ，算是BST吧
bool AB[N][N];

int buildtree(int l, int r, int id)
{
    if(l == r) {
         BIT[id] = l-1;
         return l-1;
    }
    int t1, t2;
    t1 = buildtree(l, (l+r)/2, id*2);
    t2 = buildtree((l+r)/2+1, r, id*2+1);
    BIT[id] = (p[t1] >= p[t2])?  t1 : t2;
    return BIT[id];
}

void update(int a, int l, int r, int id)
{
     if(l == r) return;
     if(p[BIT[id]] < p[a]){
                   BIT[id] = a;
     }
     if(a <= (l+r)/2)update(a, l, (l+r)/2, id*2);
     else update(a, (l+r)/2+1, r, id*2+1);
     return;
}



int main(int argc, char *argv[])
{
    scanf("%d", &n);
    int a, b;
    for(int i = 0; i < n*(n-1)/2; i++){
            scanf("%d %d", &a, &b);
            AB[a][b] = true;
            p[a] ++;
    }
    buildtree(1, n, 1);
    scanf("%d", &q);
    char c;
    for(int i = 0; i < q; ){
            c = getchar();
            if(c == 'A'){printf("%d\n", BIT[1]);i++;}
            else if(c == 'C'){
                 scanf("%d %d", &a, &b);
                 i++;
                 if(AB[a][b]){
                               AB[a][b] = false;
                               AB[b][a] = true;
                               p[a] --;
                               p[b] ++;
                               update(a, 1, n, 1);
                               update(b, 1, n, 1);
                 }else{
                               AB[b][a] = false;
                               AB[a][b] = true;
                               p[b] --;
                               p[a] ++;
                               update(b, 1, n, 1);
                               update(a, 1, n, 1);
                 }
            }
    }
    
    //system("pause");
    return 0;
}
```