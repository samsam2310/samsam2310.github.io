---
title: 'HOJ::Problem : 2 - 要我寫毛阿'
tags:
  - C++
  - HOJ
  - Stack
category:
  - Code 備忘錄
  - Data Structure
date: 2014-03-26 05:44:42
---


http://hoj.twbbs.org.tw/judge/problem/view/2
括弧匹配型的經典題。

我是把它想成有編號的括弧，本來以為有重複的元素所以還想了一個乾淨的排序法，不過很可惜這題用不到...。
把每個線段編號，開始標負數，結束標正數，排序，最後從頭跑2*n，stack如果是負數就push，如果是正數就檢查編號是不是一樣(正負關係)，不一樣代表有線段還沒結束就有線段又要開始(交錯了)，輸出N。

<!--more-->



``` c++
#include <cstdlib>
#include <iostream>
#include <algorithm>
#define N 100001
using namespace std;
struct MP{
    int id,x;
}S[2*N];
int ST[N*2],st=0;
bool operator<(const MP a,const MP b)
{
    if(a.x==b.x)return a.id>b.id;
    return a.x<b.x;
}
int main(int argc,char *argv[])
{
    ios_base::sync_with_stdio(false);
    int t,n;
    cin>>t;
    while(t--){
        cin>>n;
        st=0;
        for(int i=0;i<n;i++){
            cin>>S[i].x>>S[i+n].x;
            S[i].id=-i-1;
            S[i+n].id=i+1;
        }
        sort(S,S+2*n);
        //for(int i=0;i<2*n;i++)cout<<S[i].x<<' '<<S[i].id<<endl;
        for(int i=0;i<2*n;i++){
            if(S[i].id>0){
                if(ST[st-1]==S[i].id)ST[st--];
                else {
                    cout<<"N\n";
                    goto end;
                }
            }else{
                ST[st++]=-S[i].id;
            }
        }
        cout<<"Y\n";
        end:;
    }
    return 0;
}
```