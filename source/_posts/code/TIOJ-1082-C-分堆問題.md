---
title: 'TIOJ::1082 . C.分堆問題'
tags:
  - C++
  - TIOJ
  - NPSC
category:
  - Code 備忘錄
  - Math
date: 2014-12-15 08:46:28
---


http://tioj.infor.org/problems/1082

神奇的小題目....，理解之前覺得很難。

<!--more-->

題目給妳很多堆石頭，問你最少要搬幾顆石頭才能讓每一堆石頭都只有白色或是黑色。不可以搬到其他地方或是銷毀石頭。

首先如果**只有一堆石頭**，是黑白就不用搬，如果黑白混合那你也搬不了所以輸出 -1。

兩堆以上的情況，一般情況，只要把黑石頭或白石頭中**少的那一種**一到另外一堆去(總有一堆石頭你可以堆在那)，就可以用最少步數達到結果，所以把每一堆少的那一種加起來就結束了。

特別情況，**小的剛好都是白色或黑色**，基本上兩個情況是一樣的，你會發現你每辦法只搬少的那一種，你必須**犧牲其中一堆去搬大的那一堆**，你才有一堆可以用來堆那群比較少的石頭。我們觀察到兩堆石頭可以互相交換黑白石頭，要移動的石頭數分別是:
  假設 X1 X2 和 Y1 Y2兩堆石頭，很顯然的交換石頭的代價就是 min(X1+Y2，X2+Y1)，也可以改成 min(|X1 -X2| ，|Y1-Y2|)(前提是黑色都比較小或是白色都比較小)，然後我們可以用迴圈O(n)找出最好的那堆，讓那堆移動多的那種石頭，其他跟之前一樣移動小的那一組。



``` c++
#include <iostream>
#define N 1000001
#define F(n) Fi(i,n)
#define Fi(i,n) Fl(i,0,n)
#define Fl(i,l,n) for(int i=l;i<n;i++)
#define abs(x) ((x)>0?(x):(-(x)))
using namespace std;
int D[N][2];
bool E[N];
int main(){
    ios_base::sync_with_stdio(false);
    int n;
    while(cin>>n,n){
        int tag=0;bool tt=true;
        F(n)cin>>D[i][0]>>D[i][1],E[i]=D[i][0]>D[i][1],
            tag+=E[i],tt&=D[i][0]||D[i][1];
        if(n==1){
            cout<<((!D[0][0]||!D[0][1])?0:-1)<<endl;
            continue;
        }
        if(tt&&(tag==0||tag==n)){
            int tmp=0;
            F(n)if(abs(D[tmp][0]-D[tmp][1])>=abs(D[i][0]-D[i][1]))tmp=i;
            E[tmp]^=1;
        }
        int ans=0;
        F(n)ans+=D[i][E[i]];
        cout<<ans<<endl;
    }
}
```