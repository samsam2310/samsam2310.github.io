---
title: 'HOJ::1 - Breakfast'
tags:
  - C++
  - HOJ
category:
  - Code 備忘錄
  - Math
date: 2014-03-06 04:51:59
---


http://hoj.twbbs.org.tw/judge/problem/view/1
這算是一題數學題。比想像中的難.........

<!--more-->

題目要用一些吐司做三明治，可以用5片或3片做一個三明治，要求出剛好花完全部吐司至少要做幾個三明治(5和3都可以)。
這題分成幾個 case
1.五的被數，直接除5
2.三的倍數，必須先除15，因為一次用5片吐司比較快用完，剩下的餘數再除3。
3.如果上面兩個都不是，但剩下的吐司多於5片，減5片然後總數加1，進入迴圈。因為像 8、11、14、17這種數字，會不符合上面兩個case，但仔細觀察就會發現他們等於 5+3、5+3+3、5+3+3+3、5+3+3+3+3，這樣直接除或是直接取mod都會爛掉(自己想一想就知道了)
4.如果一直不符合1.2，吐司已經扣到少於5了，那代表無解，跳出迴圈。



``` c++
#include <cstdlib>
#include <iostream>
using namespace std;
int main(int argc,char *argv[])
{
    ios_base::sync_with_stdio(false);
    //ss:;
    int a;
    while(cin>>a){
            int sum=0;
            while(a!=0){
                    if(a%5==0){
                            sum+=a/5;
                            a=0;
                    }
                    if(a%3==0){
                            sum+=a%15/3;
                            sum+=a/15*3;
                            a=0;
                    }
                    if(a>5){
                            a-=5;
                            sum++;
                    }else{
                            break;
                    }
            }
            if(a==0)cout<<sum<<endl;
            else cout<<"This is Kongming's Trap!!!\n";
    }
    //goto ss;
    return 0;
}
```