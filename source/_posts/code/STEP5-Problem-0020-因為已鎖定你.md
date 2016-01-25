---
title: 'STEP5::Problem 0020 : 因為已鎖定你'
tags:
  - C++
  - Step5
category:
  - Code 備忘錄
  - Math
date: 2014-03-04 03:05:49
---


http://web2.ck.tp.edu.tw/~step5/probdisp.php?pid=0020
這題算是數學題。給你圓上每個點之間的距離，任意4個點可以連成一個矩形，問你最多有幾個矩形。

<!--more-->

其實觀察一下後會發現，會組成矩形的點，一定在圓的直徑上，也就是直徑兩端要是有點的話，這樣會形成一組(cu++)，最後 cu*(cu-1)/2就是全部矩形的數量(因為每兩組對角線可以組成一個矩形)。至於是不是在直徑上只要兩點間的區間和等於全部的長度(圓周)的一半就可以了。這可以O(n)算出來。如果總長度(圓周)是奇數直接輸出0(兩點間的距離都是整數，所以不會有點在直徑上)。



``` c++
#include <cstdlib>
#include <iostream>
#define N 500001
#define LL long long
using namespace std;
LL SU[N];
int main(int argc,char *argv[])
{
    ios_base::sync_with_stdio(false);
    int t,n;
    cin>>t;
    while(t--){
            fill(SU,SU+N,0);
            cin>>n;
            for(int i=1;i<=n;i++){
                    cin>>SU[i];
                    SU[i]+=SU[i-1];
            }
            LL p1=0,p2=1,cu=0;
            if(SU[n]%2==1){cout<<"0\n";goto zero;}
            while(p2<n-1){
                    while(SU[p2]-SU[p1]<SU[n]/2&&p2<n-1)p2++;
                    while(SU[p2]-SU[p1]>SU[n]/2)p1++;
                    if(SU[p2]-SU[p1]==SU[n]/2){
                            //cout<<p1<<" "<<p2<<" RR\n";
                            cu++;
                            p1++;
                    }
            }
            //cout<<cu<<" FF\n";
            cout<<cu*(cu-1)/2<<endl;
            zero:;
    }
    return 0;
}
```