---
title: 'STEP5::Problem 0149 : 轎夫'
tags:
  - C++
  - Step5
  - LCA
  - MST
category:
  - Code 備忘錄
  - Graph
date: 2014-02-19 04:06:50
---


http://web2.ck.tp.edu.tw/~step5/probdisp.php?pid=0149
這題題目是有權無項圖，要求圖中兩點間的路徑最貴的那一條邊最便宜的值。
也就是找出MST，在用LCA找出兩點之間的路徑，答案就是這條路徑上最貴的那條邊。
用vecotor存好後，priority_queue找出MST，再找LCA，我用no[]紀錄祖先，H[]紀錄深度，coco[]紀錄權重

<!--more-->


``` c++
#include <cstdio>
#include <cstdlib>
#include <iostream>
#include <vector>
#include <queue>
#define N 200001
#define LL long long
using namespace std;
struct MP{int a,b,c;};
inline bool operator<(const MP a,const MP b){
       return (a.c>b.c)?true:false;
}
vector<int> mp[N];
bool wed[N];
priority_queue<MP> qq;
int coco[N],no[N],H[N];
int fin(int a,int b)
{
    if(H[a]>H[b]){int tmp=a;a=b;b=tmp;}
    int M=0,h=H[b]-H[a];
    for(int i=0;i<h;i++){
        if(coco[b]>M)M=coco[b];
        b=no[b];
    }
    while(a!=b){
        if(coco[a]>M)M=coco[a];
        if(coco[b]>M)M=coco[b];
        a=no[a];b=no[b];
    }
    return M;
}
int main(int argc,char *argv[])
{
    int v,e,q,a,b,c;
    scanf("%d %d %d",&v,&e,&q);
    for(int i=0;i<e;i++){
            scanf("%d %d %d",&a,&b,&c);
            mp[a].push_back(b);
            mp[a].push_back(c);
            mp[b].push_back(a);
            mp[b].push_back(c);
    }
    wed[1]=1;
    for(int i=0;i*2<mp[1].size();i++)
            qq.push((MP){1,mp[1][i*2],mp[1][i*2+1]});
    while(!qq.empty()){
         while(wed[qq.top().b]){qq.pop();if(qq.empty())break;}
         if(qq.empty())break;
         int bb=qq.top().b;
         wed[qq.top().b]=1;
         no[qq.top().b]=qq.top().a;
         H[qq.top().b]=H[qq.top().a]+1;
         coco[qq.top().b]=qq.top().c;
         //printf("CA %d %d %d\n",qq.top().a,qq.top().b,qq.top().c);
         qq.pop();
         for(int i=0;i*2<mp[bb].size();i++){
                 if(!wed[mp[bb][i*2]])
                 qq.push((MP){bb,mp[bb][i*2],mp[bb][i*2+1]});
                 //printf("BB %d %d %d\n",bb,mp[bb][i*2],mp[bb][i*2+1]);
         }
     }
     //puts("WW");
     int aa,bb;
     //for(int i=0;i<5;i++){printf("%d-%d ",i,ft[i]);}
     while(q--){
                scanf("%d %d",&aa,&bb);
                printf("%d\n",fin(aa,bb));
                //printf("E:%d\n",fin(0,tim-1,aa,bb,1));
     }
     //system("pause");
     return 0;
}
```