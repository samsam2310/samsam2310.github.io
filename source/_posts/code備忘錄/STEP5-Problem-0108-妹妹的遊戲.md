---
title: 'STEP5::Problem 0108 : 妹妹的遊戲'
tags:
  - C++
  - Step5
  - BFS
category:
  - Code 備忘錄
  - Graph
date: 2014-02-19 05:07:29
---


http://web2.ck.tp.edu.tw/~step5/probdisp.php?pid=0108
這題是基礎的樹的BFS(其實本來想寫DFS.....)，跑完後在回朔路徑並加總。
寫的很難看..........

<!--more-->



``` c++
#include <cstdio>
#include <cstdlib>
#include <iostream>
#include <cstring>
#include <queue>
#define M 1000001
#define DR 1
using namespace std;

std::queue<int>noque,staque;

int n,m,q,viu[M],st[2*M][3],ste=-1,nno[M+1][2]={0};

int _walk(int sta,int en,int no,int now,int dr,int lon)//BFS
{
    nno[sta][0]=no;
    nno[sta][1]=lon;
    if(sta==en)return 1;
    //printf("AA:%d\n",sta);
    if(now==dr){
            noque.push(no);
            staque.push(sta);
            return -1;
    }
    int p=viu[sta],c=-1;
    while(p!=-1){
            if(st[p][0]==en){
                          nno[st[p][0]][0]=sta;
                          nno[st[p][0]][1]=st[p][1];
                          return 1;
            }
            else p=st[p][2];
    }
    p=viu[sta];
    while(p!=-1){
            if(st[p][0]!=no)c=_walk(st[p][0],en,sta,now+1,dr,st[p][1]);
            if(c==-1)p=st[p][2];
            else return 1;
    }    
    return -1;
}

void _v(int a,int b,int c)//用來建立反向邊
{
       if(viu[a]==-1){
              viu[a]=ste+1;
              st[++ste][0]=b;st[ste][1]=c;
       }else{
              int p=st[viu[a]][2];
              if(p==-1) st[viu[a]][2]=ste+1;
              else{
                   while(st[p][2]!=-1)p=st[p][2];
                   st[p][2]=ste+1;
              }
              st[++ste][0]=b;st[ste][1]=c;
       }
}

int tt(int en)//路徑總和
{
    int sum=0;
    while(en!=-1){
              sum+=nno[en][1];
              en=nno[en][0];
              //printf("sum:%d\n",sum);
    }
    return sum;
} 


int main(){
    for(int i=0;i<M;i++){viu[i]=-1;st[i][2]=-1;}
    for(int i=M;i<2*M;i++){st[i][2]=-1;}
    scanf("%d %d",&n,&m);
    for(int i=0;i<m;i++){
           int a,b,c;
           scanf("%d %d %d",&a,&b,&c);
           
           if(viu[a]==-1){
                  viu[a]=ste+1;
                  st[++ste][0]=b;st[ste][1]=c;
                  _v(b,a,c);
           }else{
                  int p=st[viu[a]][2];
                  if(p==-1) st[viu[a]][2]=ste+1;
                  else{
                       while(st[p][2]!=-1)p=st[p][2];
                       st[p][2]=ste+1;
                  }
                  st[++ste][0]=b;st[ste][1]=c;
                  _v(b,a,c);
           }
    }
    /*
    for(int i=0;i<ste+1;i++)
         printf("VVV %d\n",viu[i]);
    for(int i=0;i<ste+1;i++)
         printf("TTT %d %d %d\n",st[i][0],st[i][1],st[i][2]);
    */
    
    
    int a,b,t,dd,c=-1,dr=DR;
    scanf("%d %d",&a,&b);
    t=_walk(a,b,c,1,dr,0);
    while(t==-1){
             dr+=DR;
             dd=noque.size();
             while(dd--){
                           a=staque.front();
                           c=noque.front();
                           //printf("A:%d\n",a);
                           staque.pop();
                           noque.pop();
                           t=_walk(a,b,c,dr-DR,dr,nno[a][1]);
                           if(t!=-1)break;
             }
    }
    
    t=tt(b);
    printf("%d\n",t*2);
    /*
    for(int i=0;i<ste+1;i++)
         printf("nno:%d %d\n",nno[i][0],nno[i][1]);    
    */
    //system("pause");
    return EXIT_SUCCESS;
}
```