---
title: 'TOJ::46 / 染色遊戲'
tags:
  - C++
  - TOJ
  - BFS
category:
  - Code 備忘錄
  - DP
date: 2014-03-25 05:34:25
---


http://2014.sprout.csie.org/oj/pro/46/
這題有點難。

<!--more-->

基本上從三個點BFS，紀錄狀態時，我用一個整數代表現在的顏色，queue裡的資料記錄了位置、顏色、時間，當時間變大(不同時間擴散出去的)，就先暫停並記錄最大值(用陣列一次紀錄所有顏色的最大值)，BFS在把那一格加上自己的顏色時，同時紀錄自己走過這一格，因為顏色不一樣，不會干擾其他顏色，至於數量則是在擴散時判斷增加的顏色和減少的顏色來維護，要注意滴管那三格一開始的顏色。顏色的紀錄是用類似二進位的感覺(剛好可以混色)。



``` c++
#include <cstdio>
#include <cstdlib>
#include <iostream>
#include <queue>
#define XX x+X[i]
#define YY y+Y[i]
#define N 1002
using namespace std;
struct MP{
    int x,y,tm,col;
};
queue<MP>qq;
int cot[8],MX[8],MAP[N][N],X[8]={-1,-1,0,1,1,1,0,-1},Y[8]={0,1,1,1,0,-1,-1,-1},t,n;
void BFS()
{
    int up=0;
    while(!qq.empty()){
        int x=qq.front().x,y=qq.front().y,tm=qq.front().tm,col=qq.front().col;
        //printf("EE:X:%d Y:%d TM:%d col:%d\n",x,y,tm,col);
        if(tm>up){
            for(int i=0;i<8;i++)if(cot[i]>MX[i])MX[i]=cot[i];
            up=tm;
            //for(int i=1;i<8;i++)printf("%d ",cot[i]);
            system("pause");
        }
        qq.pop();
        for(int i=0;i<8;i++){
            if((XX>=0&&XX<n)&&(YY>=0&&YY<n)){
                //printf("RR:%d\n",MAP[XX][YY]&col);
                if((MAP[XX][YY]&col)==0){
                    cot[MAP[XX][YY]]--;
                    MAP[XX][YY]|=col;
                    cot[MAP[XX][YY]]++;
                    qq.push((MP){XX,YY,tm+1,col});
                }
            }
        }
    }
    return;
}
int main(int argc,char *argv[]){
    scanf("%d",&t);
    while(t--){
        fill(MX,MX+8,0);
        fill(cot,cot+8,0);
        fill((int*)MAP,(int*)MAP+N*N,0);
        scanf("%d",&n);
        int x,y,col;
        char c;
        for(int i=0;i<3;i++){
            while(c=getchar())if(c>='A'&&c<='Z')break;
            scanf("%d %d",&x,&y);
            //printf("FF:%c %d %d\n",c,x,y);
            if(c=='R')col=4;
            else if(c=='Y')col=2;
            else col=1;
            qq.push((MP){x,y,0,col});
            MAP[x][y]=col;
        }
        cot[1]=cot[2]=cot[4]=1;
        BFS();
        while(c=getchar())if(c>='A'&&c<='Z')break;
        //printf("%c ",c);
        if(c=='R')col=4;
        else if(c=='Y')col=2;
        else if(c=='B')col=1;
        else if(c=='O')col=6;
        else if(c=='G')col=3;
        else if(c=='P')col=5;
        else if(c=='D')col=7;
        printf("%d\n",MX[col]);
    }
    return 0;
}
```