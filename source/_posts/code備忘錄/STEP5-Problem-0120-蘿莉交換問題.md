---
title: 'STEP5::Problem 0120 : 蘿莉交換問題'
tags:
  - C++
  - Step5
category:
  - Code 備忘錄
  - Else
date: 2014-02-19 05:23:57
---


http://web2.ck.tp.edu.tw/~step5/probdisp.php?pid=0120
題目要交換兩行陣列中對應的數字，使得一陣列中數字不重複。
方法是先將兩個陣列並排排好，兩個兩個連起來，再把一樣的數字連起來，會連成很多環或是鍊，在任意一條上依序標上010101....

<!--more-->

標完後觀察其中一個陣列中0或1的數量，較少的便是需要交換的次數(每一條鍊或環是分開處理的)，把所有鍊或環的交換次數加總，結束~

``` c++
#include <cstdio>
#include <cstdlib>
#include <iostream>
#include <vector>
#include <queue>
using namespace std;

vector<int> lin[100001],A,B;
queue<int>  que;

int n,loli[50001][2];
bool loli2[50001]={0},br;

int link(int x,int t,int a,bool br)
{
    if(br)return (A[t]<B[t])? A[t]:B[t];
    else loli2[x]=1;
    
    if(a==0)A[t]+=1;
    else B[t]+=1;
    //printf("a:%d\n",a);
    int p=loli[x][(a+1)%2],p2=0;
    if(lin[p][p2]==x)p2+=2;
    
    if(lin[p][p2]==-1)return (A[t]<B[t])? A[t]:B[t];
    
    return link(lin[p][p2],t,lin[p][p2+1],loli2[lin[p][p2]]);
}

int fin(int x,int t,int a,bool br,bool *l)
{
    if(br)return 0;
    else loli2[x]=1;
    
    int p=loli[x][(a+1)%2],p2=0,c;
    if(lin[p][p2]==x)p2+=2;
    
    if(lin[p][p2]==-1){
                loli2[x]=0;
                *l=(a+1)%2;
                return x;
    }    
    c=fin(lin[p][p2],t,lin[p][p2+1],loli2[lin[p][p2]],l);
    loli2[x]=0;
    if(c) return c;
    else return 0; 
}


int main(int argc,char *argv[])
{
    
    scanf("%d",&n);
    for(int i=1;i<n+1;i++){
            int tmp;
            scanf("%d",&tmp);
            loli[i][0]=tmp;
            lin[tmp].push_back(i);
            lin[tmp].push_back(0);
            if(lin[tmp].size()==2)que.push(tmp);
    }
    for(int i=1;i<n+1;i++){
            int tmp;
            scanf("%d",&tmp);
            loli[i][1]=tmp;
            lin[tmp].push_back(i);
            lin[tmp].push_back(1);
            if(lin[tmp].size()==2)que.push(tmp);
    }
    
    for(int i=que.front();!que.empty();i=que.front()){
            if(lin[i].size()==2){
                    lin[i].push_back(-1);
                    lin[i].push_back(-1);
            }
            que.pop();
    }
    int t=0,sum=0,c;
    bool l;
    for(int i=1;i<n+1;i++)
            if(!loli2[i]){
                    A.push_back(0);
                    B.push_back(0);
                    c=fin(i,t,0,0,&l);
                    if(c) sum+=link(c,t,l,0);
                    else sum+=link(i,t,0,0);
                    t++;
                    //link(i,t++,0,0);
            }
    //printf("AB:%d %d\n",A[0],B[0]);
    //printf("AB:%d %d\n",A[1],B[1]);
           
    printf("%d\n",sum);
    
    //system("pause");
    return EXIT_SUCCESS;
}
```