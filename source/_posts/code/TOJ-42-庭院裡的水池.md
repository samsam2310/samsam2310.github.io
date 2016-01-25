---
title: 'TOJ::42 / 庭院裡的水池'
tags:
  - C++
  - TOJ
  - BFS
category:
  - Code 備忘錄
  - Graph
date: 2014-03-25 05:37:14
---


http://2014.sprout.csie.org/oj/pro/42/
這題就是先寫一個BFS的函數，然後N^2跑一遍，如果那格式true(還沒走過)，而且那一格是水池，那就對他BFS，把跟他連在一起的都變成false，接著紀錄總共BFS了幾次就好。

<!--more-->



``` c++
#include <cstdio>
#include <cstdlib>
#include <iostream>
#include <queue>
#define N 1010
using namespace std;
struct MP{
    int x,y;
};
bool wed[N][N];
char MAP[N][N];
int X[4]={-1,0,1,0},Y[4]={0,1,0,-1};
void BFS(MP now){
    queue<MP>qq;
    qq.push(now);
    while(!qq.empty()){
            int x=qq.front().x,y=qq.front().y;
            qq.pop();
            for(int i=0;i<4;i++){
                    if(wed[x+X[i]][y+Y[i]]){
                            wed[x+X[i]][y+Y[i]]=false;
                            if(MAP[x+X[i]][y+Y[i]]=='.')qq.push((MP){x+X[i],y+Y[i]});
                    }
            }
    }
}
int main(int argc,char *argv[])
{
    int h,w,t;
    scanf("%d",&t);
    while(t--){
            fill((bool*)wed,(bool*)wed+N*N,true);
            fill((char*)MAP,(char*)MAP+N*N,'#');
            scanf("%d %d",&h,&w);
            for(int i=1;i<=h;i++)
                    for(int j=1;j<=w;j++)
                            while(scanf("%c",&MAP[i][j]))
                                    if(MAP[i][j]=='.'||MAP[i][j]=='#')break;
            int sum=0;
            for(int i=1;i<=h;i++)
                    for(int j=1;j<=w;j++)
                            if(wed[i][j]&&MAP[i][j]=='.'){
                                    sum++;
                                    BFS((MP){i,j});
                            }
            printf("%d\n",sum);
    }
    return 0;
}
```