---
title: 'HOJ::Problem : 48 - 買醬油I'
tags:
  - C++
  - HOJ
category:
  - Code 備忘錄
  - DP
date: 2014-03-13 04:55:21
---


http://hoj.twbbs.org.tw/judge/problem/view/48
這是醬油系列題第一題........題目有一個h*w的方格，要我們找出依照上面指示會走到哪裡

<!--more-->

依照題目轉換一下，從起點(1,1)開始走，規定有兩點
1往右，0往下
走過一次0變1，1變0
題目給你n天前的狀態，問你第n天會走到哪裡。

我們不用真的走過n天才會知道n天之後的狀態，因為我們知道：走過偶數次的話狀態不變，奇數次的話就會改變，而且每次走完剛好有一半往右、一半往下，如果是奇數就看原本是往哪裡，那個方向會多一次。
由此可知，一開始起點有n次(走n天)，然後用迴圈跑，把M[i][j]的值分成一半加到下面和右邊的方格，超出去的就忽略它，並改變地圖的狀態(奇數要改變)，跑完後就得到第n天的狀態了。最後照地圖走一遍。



``` c++
#include <cstdlib>
#include <iostream>
#define N 1001
using namespace std;
bool MAP[N][N];
int M[N][N],h,w,n;
struct MP{
    int x,y;
};
void pp(){
    for(int i=0;i<h;i++){
        for(int j=0;j<w;j++)
            cout<<M[i][j]<<' ';
        cout<<endl;
    }
    cout<<endl;
}
int main(int argc,char *argv[])
{
    ios_base::sync_with_stdio(false);
    cin>>h>>w>>n;
    for(int i=0;i<h;i++)
        for(int j=0;j<w;j++)
            cin>>MAP[i][j];
    M[0][0]=n-1;
    for(int x=0;x<h;x++)
        for(int y=0;y<w;y++){
            //cout<<endl<<"RRX:"<<x<<' '<<y;
            if(x>=h||y>=w)continue;
            if(M[x][y]%2){
                if(MAP[x][y]){
                    M[x][y+1]+=M[x][y]/2+1;
                    M[x+1][y]+=M[x][y]/2;
                }else{
                    M[x][y+1]+=M[x][y]/2;
                    M[x+1][y]+=M[x][y]/2+1;
                }
                MAP[x][y]=!MAP[x][y];
            }else{
                M[x][y+1]+=M[x][y]/2;
                M[x+1][y]+=M[x][y]/2;
            }
            // cout<<endl<<"FFX:"<<x<<' '<<y<<endl;
            // pp();
            M[x][y]=0;
        }
    // for(int i=0;i<h;i++){
    //     for(int j=0;j<w;j++)
    //         cout<<MAP[i][j];
    //     cout<<endl;
    // }
    int tx=0,ty=0;
    while(tx<h&&ty<w){
        if(MAP[tx][ty])ty++;
        else tx++;
    }
    cout<<tx+1<<' '<<ty+1<<endl;
    return 0;
}
```