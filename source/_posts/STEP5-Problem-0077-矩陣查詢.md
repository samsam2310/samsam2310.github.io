---
title: 'STEP5::Problem 0077 : 矩陣查詢'
tags:
  - C++
  - Step5
  - Binary Index Tree
category:
  - Code 備忘錄
  - Data Structure
date: 2014-02-19 04:12:57
---


http://web2.ck.tp.edu.tw/~step5/probdisp.php?pid=0077
這題是前綴和，第一次寫。
有使用到樹套樹，分別在C x1,y1 、x2+1,y2+1   的地方加上1，x1,y2+1  x2+1,y1  的地方加上-1，query就直接將左上到x,y的和%2就可以了。

<!--more-->


``` c++
#include <cstdlib>
#include <cstdio>
#include <iostream>
#define N 1010
using namespace std;

int brod[N][N], t, n;

void clear()
{
     for(int i = 0; i < N; i++)
             for(int j = 0; j < N; j++)
                     brod[i][j] = 0;
}

void update(int idx,int idy,int a)
{
     for(int i = idy; i <= n; i+=(i&(-i)))
             for(int j=idx;j<=n;j+=(j&(-j)))
                     brod[i][j]+=a;
}

int query(int idx,int idy)
{
    int sum=0;
    //printf("M:%d \n",brod[idx][idy]);
    for(int i = idy; i > 0; i-=(i&(-i)))
            for(int j = idx; j > 0; j-=(j&(-j)))
                    sum+=brod[i][j];
    return sum;
}

int main(int argc,char *argv[])
{
    scanf("%d", &t);
    while(t--){
            int x1, x2, y1, y2, p;
            char c = '\n';
            scanf("%d %d", &n, &p);
            clear();
            while(p){
                  c = getchar();
                  if(c == 'C'){
                           p--;
                           scanf("%d %d %d %d", &x1, &y1, &x2, &y2);
                           update(x1, y1, 1);
                           if(y2 + 1 <= n)
                                 update(x1, y2+1, -1);
                           if(x2 + 1 <= n)
                                 update(x2+1, y1, -1);
                           if(y2 + 1 <= n && x2 + 1 <= n)
                                 update(x2+1, y2+1, 1);
                           /*for(int i = 1; i <= n; i++){
                                   for(int j = 1; j <= n; j++)
                                           printf("%d ", query(i, j)%2);
                                   puts("");
                           }*/
                           
                  }else if(c == 'Q'){
                           p--;
                           scanf("%d %d",&x1,&y1);
                           //printf("DD %d-%d-%d+%d DD",query(x2,y2),query(x1,y2),query(x2,y1),query(x1,y1));
                           printf("%d\n", (query(x1, y1))%2);
                  }  
            }
            puts("");
    }
    //system("pause");
    return 0;
}
```