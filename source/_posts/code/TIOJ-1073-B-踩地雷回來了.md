---
title: 'TIOJ::1073 . B.踩地雷回來了'
tags:
  - C++
  - TIOJ
  - NPSC
  - DFS
category:
  - Code 備忘錄
  - Search
date: 2014-12-14 08:43:09
---


http://tioj.infor.org/problems/1073

這題感覺上應該是暴搜題。可是很難寫，特例很多，基本上沒玩過踩地雷的應該不會寫|||。
TLE了很久才AC。

<!--more-->

題目說了**不會有不合法的盤面**，所以我們一開始就可以假設盤面都是對的，這樣一來，最自然的作法就是遇到空格就放，或是不放，最後看看合不合法，炸彈是不是放完了。

其中狀況有很多種，所以很麻煩，以每個空格來說，首先就是**周為都不是0**(可以是 1~8 或是空格或是炸彈)，**才可以擺炸彈**。如果左上角是 1~8 不可以空著(其實算是個小優化，雖然不一定有用XD)。

基本上狀態就切成，可以擺炸彈和不能擺炸彈，還有一種--不知道，因為如果**周圍都是還沒翻開的格子，狀態是不確定的**。

可是如果我們就這樣搜下去絕對會TLE的，於是就要優化啦！我們可以觀察到有很多格子其時是不確定的，對於這些不確定的個子我們可以隨便擺，所以就用**排列組合**去算就好，如果剩下3個空格要放2顆炸彈，就是 C(3，2)。這樣就可以讓速度快上很多，因為確定的格子不是炸彈就是數字，可能性很少，**不確定的格子才是增加遞迴次數的主要原因**。

然而**剩下幾個空格**其實很容易錯，基本上因為太複雜了所以我就建表去改囉。當有一個數字變成0的同時，她周圍的不確定格子瞬間都變成一定不是炸彈了，我們要把這些格子去掉，一開始建一個表記錄哪些格子有可能是炸彈，在遞迴的過程中一序拔掉不可能的，注意要記得還原被改過的值，因為 9*9很小所以我在遞迴裡直接開一個陣列保存原本的值。

邊界還有一些狀況要想仔細，到底時，在掃一遍確定每個數字都變成0了，記得還元變數，全域變數和區域變數分清楚，就可以過了。



``` c++
#include <iostream>
#define F(n) Fi(i,n)
#define Fi(i,n) Fl(i,0,n)
#define Fl(i,l,n) for(int i=l;i<n;i++)
#define XX x+X[i]
#define YY y+Y[i]
using namespace std;
bool SP[10][10];
char D[10][10];
int C[82][82],n,m,cnt,X[8]={-1,-1,-1,0,1,1,1,0},Y[8]={-1,0,1,1,1,0,-1,-1};
bool ck(int x,int y){
    return x<0||x>=n||y<0||y>=m;
}
int gg(int x,int y,int t){
    if(ck(x,y)||D[x][y]>8)return 0;
    D[x][y]+=t;
    int tmp=0;
    if(D[x][y]==0)F(8)if(!ck(XX,YY)&&SP[XX][YY])tmp++,SP[XX][YY]=false;
    return tmp;
}
int DFS(int x,int y,int b,int B){
    if(y==m){
        if(ck(x-1,y-1)||D[x-1][y-1]>8||D[x-1][y-1]==0)DFS(x+1,0,b,B);
        return 0;
    }
    if(x==n){
        // cout<<b<<' '<<B<<endl;
        // F(n){Fi(j,m)cout<<(char)(D[i][j]+'0');cout<<endl;}cout<<endl;
        F(n)Fi(j,m)if(D[i][j]>0&&D[i][j]<=8)return 0;
        return cnt+=C[B][b];
    }
    if(D[x][y]<=8){
        if(ck(x-1,y-1)||D[x-1][y-1]>8||D[x-1][y-1]==0)DFS(x,y+1,b,B);
        return 0;
    }
    bool tag=true,tt=true;
    F(8)tag&=ck(XX,YY)||D[XX][YY];
    F(8)tt&=ck(XX,YY)||D[XX][YY]>8;
    if(tag&&!tt){
        int tmp=0;
        bool SPB[n][m];
        F(n)Fi(j,m)SPB[i][j]=SP[i][j];
        F(8)tmp+=gg(XX,YY,-1);
        D[x][y]++;
        if(ck(x-1,y-1)||D[x-1][y-1]>8||D[x-1][y-1]==0)
            if(b>0)DFS(x,y+1,b-1,B-tmp);
        D[x][y]--;
        F(8)gg(XX,YY,1);
        F(n)Fi(j,m)SP[i][j]=SPB[i][j];
    }else if(!tag){
        bool tmp=SP[x][y];SP[x][y]=false;
        if(ck(x-1,y-1)||D[x-1][y-1]>8||D[x-1][y-1]==0)
            if(b<=B-tmp)DFS(x,y+1,b,B-tmp);
        SP[x][y]=tmp;
    }
    if(tag&&(ck(x-1,y-1)||D[x-1][y-1]>8))DFS(x,y+1,b,B);
    return 0;

}
int main(){
    ios_base::sync_with_stdio(false);
    C[0][0]=1;
    F(81)Fi(j,i+2)C[i+1][j]=C[i][j]+(j?C[i][j-1]:0);
    int b,B;
    cin>>n;
    while(cin>>n>>m>>b){
         B=cnt=0;
         F(n)cin>>D[i];
         F(n)Fi(j,m)D[i][j]-='0',SP[i][j]=D[i][j]>8,B+=SP[i][j];
         DFS(0,0,b,B);
         cout<<cnt<<endl;
    }
}
```