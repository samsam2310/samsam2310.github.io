---
title: 'STEP5::Problem 0022 : 掉落的橘子'
tags:
  - C++
  - Step5
category:
  - Code 備忘錄
  - Math
date: 2014-03-06 04:08:44
---


http://web2.ck.tp.edu.tw/~step5/probdisp.php?pid=0022
題目要我們找出最佳的位置上，在cost範圍內能加入幾個點，每加入一個點就會花費d，d是點和最佳位置的距離。

<!--more-->

基本上一段區間中的最佳位置一定是這段區間的中位數(項數是偶數個了話中間兩個數任意一個都成立)，因為中位數左右點的數量一樣，計算距離不會重疊到(左右抵銷)。
總之就是一直從右邊push點近來，並更新中位數的位置，順便更新總cost(左邊有N個點就加上N乘以中位數移動的距離，右邊有M個點就扣掉M乘以中位數移動的距離)。如果cost小於或等於題目給的上限，就記錄現在總共有幾個點(紀錄最大值)，如果cost超過就從左邊pop掉點，並更新中位數和總cost，這樣O(N)可以做完。
要注意檢查cost和更新中位數的順序和迴圈的上限。



``` c++
#include <cstdlib>
#include <iostream>
#define N 5000001
#define LL long long
using namespace std;
int R[N];
int main(int argc,char *argv[])
{
    ios_base::sync_with_stdio(false);
    int t,l,r;
    cin>>t;
    while(t--){
        LL b=0;
        cin>>r>>l>>b;
        //cout<<r<<' '<<l<<' '<<b<<" II\n";
        for(int i=1;i<=r;i++)cin>>R[i];
        int p1=1,p2=1,pm=1,le=0,re=0,ma=0;
        LL cost=0;
        while(p2<=r){
            //cout<<p1<<' '<<pm<<' '<<p2<<' '<<cost<<endl;
            if(cost<=b){
                ma=(ma>le+re+1)?  ma:le+re+1;
                //cout<<"ma :"<<ma<<endl;
                p2++;
                re++;
                if(p2<=r)cost+=R[p2]-R[pm];
                if(pm<(p1+p2)/2){
                    pm++;
                    le++;
                    cost+=(le-re)*(R[pm]-R[pm-1]);
                    re--;
                }
            }else{
                cost-=R[pm]-R[p1];
                p1++;
                le--;
                if(pm<(p1+p2)/2){
                    pm++;
                    le++;
                    cost+=(le-re)*(R[pm]-R[pm-1]);
                    re--;
                }
            }
        }
        cout<<ma<<endl;
    }
    //system("pause");
    return 0;
}
```