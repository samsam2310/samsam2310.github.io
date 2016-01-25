---
title: 'HOJ::Problem : 52 - 瀚瀚數列'
tags:
  - C++
  - HOJ
  - Stack
category:
  - Code 備忘錄
  - Data Structure
date: 2014-03-23 05:23:42
---


用類似stack的概念解。
http://hoj.twbbs.org.tw/judge/problem/view/52

<!--more-->

這題題目把費式數列變成一個進位法。詳細就不多說了。

觀察後會發現  011 --> 100， 0200--> 1001  ，先從後往前跑，如果有遇到這種狀況就轉換，並往後檢查，如果有需要就後退，不然一直往前跑，跑到最高位(陣列最前面)就結束。



``` c++
#include <cstdlib>
#include <iostream>
#define N 1000100
using namespace std;
int as[N];
void po(int i)//顯示變化狀態
{
    for(int j=0;j<10;j++)if(j==i)cout<<'|';else cout<<' ';
    cout<<endl;
    for(int j=0;j<10;j++)cout<<as[j];
    cout<<endl;
}
int main(int argc,char *argv[])
{
    ios_base::sync_with_stdio(false);
    int m,n,tmp;
    cin>>n;
    for(int i=0;i<n;i++){
            cin>>as[i];
    }
    cin>>m;
    for(int i=0;i<m;i++){
            cin>>tmp;
            as[i]+=tmp;
    }
    int i=N-10;
    //int i=20;
    while(i>=0){
            if(i>0&&as[i]==1&&as[i-1]>0){
                    as[i]--;as[i-1]--;
                    as[i+1]++;
                    i+=2;
            }
            else if(as[i]>1){
                    if(i>1)as[i-2]++;
                    else if(i==1)as[i-1]++;
                    as[i+1]++;
                    as[i]-=2;
                    i+=2;
            }
            else i--;
            //po(i);
    }
    tmp=1;
    for(int i=0;i<N;i++)if(as[i])tmp=i+1;
    cout<<tmp;
    for(int i=0;i<tmp;i++)cout<<' '<<as[i];
    cout<<endl;
     // system("pause");
    return 0;
}
```