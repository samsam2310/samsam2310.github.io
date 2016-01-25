---
title: 'TOJ::44 / 喵喵抓老鼠'
tags:
  - C++
  - TOJ
  - BFS
category:
  - Code 備忘錄
  - Search
date: 2014-03-25 05:36:16
---


http://2014.sprout.csie.org/oj/pro/44/
很基礎的BFS搜索，碰到老鼠就跳掉，BFS要注意不要往回走。

<!--more-->



``` c++
#include <cstdio>
#include <cstdlib>
#include <iostream>
#include <queue>
#include <cstring>
#define N 101
using namespace std;
struct MP{
    int x,y,c;
};
char MAP[N][N];
bool wed[N][N];
int X[4]={-1,0,1,0},Y[4]={0,1,0,-1};
int main(int argc,char *argv[])
{
    int n;
    scanf("%d",&n);
    while(n!=0){
            queue<MP> qq;
            for(int i=0;i<n;i++)scanf("%s",MAP[i]);
            fill((bool*)wed,(bool*)wed+N*N,true);
            for(int i=0;i<n;i++)
                    for(int j=0;j<strlen(MAP[i]);j++)
                            if(MAP[i][j]=='K'){
                                    qq.push((MP){i,j,0});
                                    wed[i][j]=false;
                                    goto start;
                            }
            start:;
            int ans=-1;
            while(!qq.empty()){
                    int x=qq.front().x,y=qq.front().y,c=qq.front().c;
                    //printf("DD:x:%d,Y:%d,C:%d\n",x,y,c);
                    qq.pop();
                    for(int i=0;i<4;i++){
                            if(wed[x+X[i]][y+Y[i]]){
                                //printf("   AA:%d\n",i);
                                    wed[x+X[i]][y+Y[i]]=false;
                                    if(MAP[x+X[i]][y+Y[i]]=='#')continue;
                                    if(MAP[x+X[i]][y+Y[i]]=='@'){
                                            ans=c+1;
                                            goto end;
                                    }
                                    if(MAP[x+X[i]][y+Y[i]]=='.')qq.push((MP){x+X[i],y+Y[i],c+1});
                            }
                    }
            }
            end:;
            if(ans!=-1)printf("%d\n",ans);
            else puts("= =\"");
            scanf("%d",&n);
    }
    return 0;
}
```