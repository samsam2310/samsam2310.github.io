---
title: 'STEP5::Problem 0019 : Ch2-8.邁向未來的分歧'
tags:
  - C++
  - Step5
category:
  - Code 備忘錄
  - Else
date: 2014-03-06 04:29:34
---


http://web2.ck.tp.edu.tw/~step5/probdisp.php?pid=0019
算是簡單的題目，拿來練習struct。

<!--more-->

題目要我們把三個數字中兩個數字變成0，一個數字變成正數，過程中不能讓任意數字變成負數，每次可以選擇其中一個數字加1，其他減1。
作法就是一直加最小的數字，直到有兩個數字值一樣，就加剩下一個數字直到其他數字都變成0。



``` c++
#include <cstdlib>
#include <iostream>
#include <vector>
using namespace std;
struct MP{
    int Li[3];
    int en(){
        if(Li[0]==Li[1]&&Li[0]==0)return 2;
        if(Li[1]==Li[2]&&Li[1]==0)return 0;
        if(Li[0]==Li[2]&&Li[0]==0)return 1;
        return -1;
    }
    int pi(){
        if(Li[0]==Li[1])return 1;
        if(Li[1]==Li[2])return 2;
        if(Li[0]==Li[2])return 0;
        return -1;
    }
    int mi(){
        if(Li[0]<Li[1]&&Li[0]<Li[2])return 0;
        if(Li[1]<Li[2]&&Li[1]<Li[0])return 1;
        else return 2;
    }
    int doi(int i){
        Li[0]--;
        Li[1]--;
        Li[2]--;
        Li[i]+=2;
    }
}my;
vector<int> dd;
char C[3]={'I','S','A'};
int main(int argc,char *argv[])
{
    //ios_base::sync_with_stdio(false);
    int i=0;
    cin>>my.Li[0]>>my.Li[1]>>my.Li[2];
    while(my.en()==-1){
            int tmp=my.pi();
            if(tmp!=-1){
                    i+=my.Li[tmp];
                    while(my.Li[tmp]--)dd.push_back((tmp+1)%3);
                    break;
            }
            tmp=my.mi();
            my.doi(tmp);
            i++;
            dd.push_back(tmp);
            //cout<<my.Li[0]<<' '<<my.Li[1]<<' '<<my.Li[2]<<endl;
            //system("pause");
    }
    cout<<i<<endl;
    for(int i=0;i<dd.size();i++)cout<<C[dd[i]];
    cout<<endl;
    return 0;
}
```