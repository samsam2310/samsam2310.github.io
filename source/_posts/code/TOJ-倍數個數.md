---
title: 'TOJ::倍數個數'
tags:
  - C++
  - TOJ
category:
  - Code 備忘錄
  - Math
date: 2014-02-25 06:00:00
---


求 A到B(A

<!--more-->

解法就是，先判斷正負性質，再全部取絕對值，然後A/x+A/y-A/(x*y/GCD(x,y))
也就是A各除以兩數再扣掉除以他們的最小公倍數，B也做一次。
如果剛剛的正負性質是AB都小於0或都大於0，就把大的減小的(我是都變成正的然後小的換到A)，如果有跨過0就加起來再加1(0也要算)。
因為AB也要算到區間裡，所以一開始A先減1。
因為公倍數有可能爆long long ，要注意。
因為XY和GCD(??,0)有可能是0，要注意不要除到0。

因為很麻煩所以if海AC掉.........



``` c++
#include <cstdio>
#include <cstdlib>
#include <iostream>
#include <algorithm>
#define LL long long
#define MM 9223372036854775807
using namespace std;
int main(int argc,char *argv[])
{
    nn:;
    LL a,b,x,y;
    int zero=0;
    scanf("%lld %lld %lld %lld",&a,&b,&x,&y);
    LL sum=0;
    if(b<0){x=abs(x);y=abs(y);int tmp=-a-1;a=-b;b=tmp;}
    else if(a>0){a--;x=abs(x);y=abs(y);}
    else {zero=1;a=abs(a);b=abs(b);x=abs(x);y=abs(y);}
    if(zero){
        if(x!=0){
            sum+=a/x+b/x;
        }
        if(y!=0){
            sum+=a/y+b/y;
        }
        //puts("AA");
        LL gg=__gcd(x,y);
        if(gg!=0){
            if((x/gg)!=0){
                //puts("bb");
                if(MM/(x/gg)>y){
                    //puts("cc");
                    LL g=(x/gg)*(y/gg)*gg;
                    if(g!=0)sum+=-a/g-b/g;
                }
            }
        }
        sum++;
    }else{
        if(x!=0){
            sum+=-a/x+b/x;
        }
        if(y!=0){
            sum+=-a/y+b/y;
        }
        LL gg=__gcd(x,y);
        if(gg!=0){
            if((x/gg)!=0){
                if(MM/(x/gg)>y){
                    LL g=(x/gg)*(y/gg)*gg;
                    if(g!=0)sum+=a/g-b/g;
                }
            }
        }
    }
    //printf("%lld %lld: ",a,b);
    printf("%lld\n",sum);
    //goto nn;
    //system("pause");
    return 0;
}
```