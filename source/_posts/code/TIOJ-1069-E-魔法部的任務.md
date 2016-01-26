---
title: 'TIOJ::1069 . E.魔法部的任務'
tags:
  - C++
  - TIOJ
  - NPSC
  - 二分圖
category:
  - Code 備忘錄
  - Graph
date: 2014-12-11 08:37:29
---


http://tioj.infor.org/problems/1069

這題是我的第一題二分圖匹配，WA超多次的......。

<!--more-->

這題題目是，你有一堆事件，會在一個時間發生，發生在一個座標，而你要派魔法師去處理他，魔法師一開始可以傳送到指定的地方處理任務，但是到外面後便只能靠開車移動，每一單位時間可以移動一單位距離，而事件之間的距離是座標的曼哈頓距離，每個魔法師如果可以趕到另外一個事件發生地就可以處理該事件，你希望用最少的人力去處理這些事件。

我們先假設，如果處理完事件I，有辦法再去處理事件J，那我們就建一條邊。這樣會建出一張DAG。然後我們要找出一些路徑覆蓋這張圖，只要用最少的路徑就代表最少人。
然後我們可以改一下圖的樣字，把一個點拆成兩個（A部份和B部份），把剛剛的路徑改成，改成事件I的A部份連到事件J的B部份，這樣我們的圖就變成一張二分圖（A組和B組），一條路徑就會變成一連串的A和B串聯。
所以現在只要再二分圖上儘量把兩邊的點連起來，就可以讓路徑數變少，也就是二分圖最大匹配。原本的點有N個的話，每有一對匹配路徑數就會少1，（N-最大匹配數）就是我們要的答案。

所以題目就變成先建圖，再做二分圖最大匹配，也就是匈牙利演算法。

匈牙利演算法的概念就是，對於所有點，遍歷每條邊，如果可以匹配就直接匹配並中斷迴圈，如過那個點以經被別人匹配了，那就較那個點去跟別人匹配看看（丟下去遞迴），如果可以匹配，那就讓那個點去跟別人匹配，自己就跟這個點匹配。如果不行，就再找下一條邊，直到全部都試過，發現不行，就回傳失敗。

要注意的是，不可以自環（會爛掉），每次DFS進去要把走過的點標記掉，不然會無線循環的詢問某個點可不可以跟別人匹配。但是每次回到最上層就要把標記清掉，每次的擴充都要清掉一次。

其實code很短，概念抓到就會AC了。不過概念需要思考很久就是了.....。



``` c++
#include <iostream>
#include <cstring>
#include <vector>
#define N 1001
#define F(n) Fi(i,n)
#define Fi(i,n) Fl(i,0,n)
#define Fl(i,l,n) for(int i=l;i<n;i++)
#define abs(x) ((x)>0?(x):(-(x)))
using namespace std;
int B[N],cnt;
bool wed[N];
struct MP{
    int t,x,y;
}D[N];
bool DFS(vector<int>*G,int x){
    wed[x]=true;
    F(G[x].size())if(!wed[B[G[x][i]]-1]){
        if(!B[G[x][i]]||DFS(G,B[G[x][i]]-1)){
            if(!B[G[x][i]])cnt++;
            B[G[x][i]]=x+1;
            return true;
        }
    }
    return false;
}
main(){
    int n;
    cin>>n;
    while(cin>>n){
        vector<int>G[N];
        F(n)cin>>D[i].t>>D[i].x>>D[i].y;
        F(n)Fi(j,n)if(i!=j&&abs(D[i].x-D[j].x)+abs(D[i].y-D[j].y)+D[i].t<=D[j].t){
            G[i].push_back(j);
        }
        memset(B,0,sizeof(B));
        cnt=0;
        F(n)DFS(G,i),memset(wed,0,sizeof(wed));
        cout<<(n-cnt)<<endl;
    }
}
```