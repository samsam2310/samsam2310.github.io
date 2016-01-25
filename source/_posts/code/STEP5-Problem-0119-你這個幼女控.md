---
title: 'STEP5::Problem 0119 : 你這個幼女控'
tags:
  - C++
  - Step5
  - DFS
category:
  - Code 備忘錄
  - Search
date: 2014-02-19 05:18:12
---


http://web2.ck.tp.edu.tw/~step5/probdisp.php?pid=0119
其實應該不算DP，應該是遞迴吧。(分類太多會很煩...

<!--more-->

總之，這題是8皇后問題。有人直接跑出全部的結果再一個一個比......雖然我覺得是好方法啦，可是我不會八皇后ㄏㄏ........

關鍵是，用四組boolen分別記錄｜－＼／的狀態，直接遞迴下去，如果卡了就重來，跑完放不了8顆棋子就輸出NO。

### 2016/1/25
我現在覺得是DFS了XDD

``` c++
#include <stdio.h>
#include <stdlib.h>
 
int n;
int c[8][8],al[8]={0},ak[8]={0},b1[15]={0},b2[15]={0};

int pu(int);
void pfor(int[][8]);
void qq(int,int,int);
void remo(int[],int);

int main(int argc, char *argv[])
{
  scanf("%d",&n);
  for(int i=0;i<n;i++){
          int pp=1;
          remo(al,8); remo(ak,8); remo(b1,15); remo(b2,15);
          for(int l=0;l<8;l++)
                 for(int k=0;k<8;k++){
                          scanf("%d",&c[l][k]);
                          if(c[l][k]==1)
                                   if(al[l]==0&&ak[k]==0)qq(l,k,1);
                                   else pp=-1;
                 }
          if(pp==-1)printf("-1\n");
          else if(pu(0)==-1)printf("-1\n");
          else  pfor(c);
  }
  //system("pause");    
  return 0;
}



int pu(int dr)
{
    //printf("dr:%d al[dr]:%d\n",dr,al[dr]);
    if(dr==7){
           //puts("!!!");pfor(c);puts("!!!");
           if(al[dr]==1)return 1;
           else{
                    for(int i=0;i<8;i++){
                            if(ak[i]>0)continue;
                            if(b1[dr+i]>0||b2[7-i+dr]>0)continue;
                            qq(dr,i,1);
                            return 1;
                    }
                    return -1;
           }
    }
    if(al[dr]>0)return pu(dr+1);
    for(int i=0;i<8;i++){
            if(ak[i]>0)continue;
            if(b1[dr+i]>0||b2[7-i+dr]>0)continue;
            qq(dr,i,1);
            if(pu(dr+1)==-1)qq(dr,i,0);
            else return 1;
    }
    return -1;
}

void remo(int T[],int t)
{
     for(int i=0;i<t;i++)
             T[i]=0;
     return;
}

void qq(int x,int y,int Q)
{
     c[x][y]=Q;
     al[x]=Q;
     ak[y]=Q;
     b1[x+y]=Q;
     b2[7-y+x]=Q;
     return;
}

void pfor(int W[][8])
{
     for(int h=0;h<8;h++)
             printf("%d %d %d %d %d %d %d %d\n",W[h][0],W[h][1],W[h][2],W[h][3],W[h][4],W[h][5],W[h][6],W[h][7]);
     return;
}
```