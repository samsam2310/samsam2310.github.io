---
title: 'STEP5::Problem 0071 : 卡卡跑丁車'
tags:
  - C++
  - Step5
  - NPSC
category:
  - Code 備忘錄
  - DP
date: 2014-03-23 05:21:02
---


動態規劃。
http://web2.ck.tp.edu.tw/~step5/probdisp.php?pid=0071

<!--more-->

題目給了一段直線賽道，要找出最快通過賽道的時間，每段賽道間有一段彎道，彎道可以甩尾並累積甩尾次數，但是時間加倍(5變10)，三次甩尾可以兌換一個加速器，一段直線賽道可以使用一個加速器並用一半的時間通過，加速器最多只能累積兩個。

狀態改成，一開始第一段直線要用原來的時間通過，DP[0][0]設為第一段時間，之後DP[i][j]代表第 i 條直線賽道加前一條彎道，j 帶表甩了幾次尾。狀態可以從小的 j 甩尾上來，或是 甩尾加之前兩次兌換一次加速，再跟直接往前開作比較，for迴圈跑完最後一行最小值即是答案。



``` c++
#include <cstdlib>
#include <iostream>
#define N 10001
#define LL long long
using namespace std;
LL DP[N][7],R[N];
int main(int argc,char *argv[])
{
    ios_base::sync_with_stdio(false);
    int n;
    cin>>n;
    for(int i=0;i<n;i++)cin>>R[i];
    //for(int i=0;i<5;i++)cout<<"  FF "<<R[i];
    fill((LL*)DP,(LL*)DP+N*7,-1);
    DP[0][0]=R[0]*2;
    for(int i=1;i<7;i++)DP[0][i]=-1;
    for(int i=1;i<n;i++){
        for(int j=0;j<7;j++){
            if(DP[i-1][j]!=-1)DP[i][j]=DP[i-1][j]+5+R[i]*2;
        }
        for(int j=0;j<7;j++){
            // if(j+3<6&&DP[i-1][j+3]!=-1){
            //  DP[i][j]=(DP[i][j]<DP[i-1][j+3]+5+R[i]&&DP[i][j]!=-1)? DP[i][j]:DP[i-1][j+3]+5+R[i];
            // }
            if(j+2<6&&DP[i-1][j+2]!=-1){
                DP[i][j]=(DP[i][j]<DP[i-1][j+2]+10+R[i]&&DP[i][j]!=-1)? DP[i][j]:DP[i-1][j+2]+10+R[i];
            }
            if(j-1>=0&&DP[i-1][j-1]!=-1){
                DP[i][j]=(DP[i][j]<DP[i-1][j-1]+10+R[i]*2&&DP[i][j]!=-1)? DP[i][j]:DP[i-1][j-1]+10+R[i]*2;
            }
        }
    }
    // for(int i=0;i<7;i++){
    //  for(int j=0;j<n;j++)cout<<' '<<DP[j][i];
    //  cout<<endl;
    // }
    LL mi=(LL)1e18;
    for(int i=0;i<7;i++)
        if(mi>DP[n-1][i]&&DP[n-1][i]!=-1)mi=DP[n-1][i];
    cout<<mi<<endl;
    // system("pause");
    return 0;
}
```