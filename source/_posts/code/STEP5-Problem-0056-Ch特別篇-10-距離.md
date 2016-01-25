---
title: 'STEP5::Problem 0056 : Ch特別篇-10.距離'
tags:
  - C++
  - Step5
category:
  - Code 備忘錄
  - Math
date: 2014-03-23 05:22:37
---


有點數學的題目。
http://web2.ck.tp.edu.tw/~step5/probdisp.php?pid=0056
題目給你N個點，問這N個點距離的平方和，也就是每個點會有N(N-1)/2條邊，這些邊的平方和。

<!--more-->

首先，這題先拆成X和Y軸，最後加起來就可以。再來用類似求區間和的感覺，只是要轉換一下。先照座標排序，O(n)跑出座標差和前綴和，順便紀錄前綴和的平方的總和，做完後這個總和就是第一個點到所有點距離的平方和，再來推到第二個點，用和的平方公式，他們的到所有點的距離的平方和(去掉跟第一個點的)就是本的總和扣掉 點1到點2 距離的平方(a平方)在去掉2*這段長度*後面的長度(2ab)，剩下的就是後面點的平方和(b平方)，用分配律換一下就可以O(n)求解。



``` c++
#include <cstdlib>
#include <iostream>
#include <algorithm>
#define N 1000001
#define LL long long
#define pow(x) ((x)*(x))
using namespace std;
LL xdt[N],ydt[N],xpf[N],ypf[N];
int main(int argc,char *argv[])
{
    ios_base::sync_with_stdio(false);
    LL n;
    cin>>n;
    for(LL i=0;i<n;i++)cin>>xdt[i]>>ydt[i];
    sort(xdt,xdt+n);
    sort(ydt,ydt+n);
    xpf[0]=xdt[0]=xdt[1]-xdt[0];
    ypf[0]=ydt[0]=ydt[1]-ydt[0];
    n--;
    for(LL i=1;i<n;i++){
        xdt[i]=xdt[i+1]-xdt[i];
        ydt[i]=ydt[i+1]-ydt[i];
        xpf[i]=xdt[i]+xpf[i-1];
        ypf[i]=ydt[i]+ypf[i-1];
    }
    //for(int i=0;i<n;i++)cout<<xdt[i]<<'('<<xpf[i]<<") "<<ydt[i]<<'('<<ypf[i]<<endl;
    LL xsum=0,ysum=0,xcut=0,ycut=0;
    for(LL i=0;i<n;i++){
        xsum+=pow(xpf[i]);
        xcut+=xpf[i];
        ysum+=pow(ypf[i]);
        ycut+=ypf[i];
    }
    LL ans=0;
    for(LL i=0;i<n;i++){
        ans+=xsum+ysum;
        xcut-=(n-i)*xdt[i];
        ycut-=(n-i)*ydt[i];
        xsum+=-(n-i)*pow(xdt[i])-2*xdt[i]*xcut;
        ysum+=-(n-i)*pow(ydt[i])-2*ydt[i]*ycut;
    }
    cout<<ans<<endl;
    //system("pause");
    return 0;
}
```